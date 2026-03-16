Knowledge Base for Fedora

## Fedora 43 Kinoite {#fedora_43_kinoite}

### Custom system-wide CA {#custom_system_wide_ca}

``` bash
sudo cp my-root-cert.crt /etc/pki/ca-trust/source/anchors/
sudo update-ca-trust extract
```

### Hypervisor: Use qemu & kvm {#hypervisor_use_qemu_kvm}

``` bash
# 1) Create new deployment with virtualization and Windows 11 prerequisites
sudo rpm-ostree install qemu-kvm libvirt virt-manager virt-install virt-viewer edk2-ovmf swtpm

# 2) Reboot into that deployment
systemctl reboot

# 3) Check for known bug in Fedora
grep -E '^libvirt:' /etc/group || echo "no libvirt in /etc/group"

## If output is missing your encountering the bug: group exists in /usr/lib/group but not in /etc/group, s
## Copy the system group into /etc/group. Safe append from system database into /etc
## Source: https://github.com/crc-org/crc/wiki/Fails-to-add-the-user-to-the-libvirt-group-Fedora-Silverblue/a64b0efc0a4a293ae2883e69d3881312c7409245
getent group libvirt | sudo tee -a /etc/group

## Add your user to the group (and re-login afterwards)
sudo gpasswd -a <YOUR_USER> libvirt

# 4) Allow your user to manage VMs without sudo after re-login
sudo usermod -aG libvirt $(whoami)

# 5) Start/enable the hypervisor daemon
sudo systemctl enable --now libvirtd

# 6) (Re-login so the libvirt group takes effect if you added yourself)
```

``` bash
# After the installation with rpm-ostree you will find the packages in LayeredPackages
$ sudo rpm-ostree status
State: idle
Deployments:
● fedora:fedora/42/x86_64/kinoite
                  Version: 42.20250812.0 (2025-08-12T00:32:31Z)
               BaseCommit: 5fb4697b96656c7a5809a34c143c6fdcad6ba05c1632daa4cb33d755252cbe1d
             GPGSignature: Valid signature by B0F4950458F69E1150C6C5EDC8AC4916105EF944
          LayeredPackages: edk2-ovmf libvirt qemu-kvm swtpm virt-install virt-manager virt-viewer

  fedora:fedora/42/x86_64/kinoite
                  Version: 42.20250812.0 (2025-08-12T00:32:31Z)
                   Commit: 5fb4697b96656c7a5809a34c143c6fdcad6ba05c1632daa4cb33d755252cbe1d
             GPGSignature: Valid signature by B0F4950458F69E1150C6C5EDC8AC4916105EF944
```

## Flatpak

### Flatpak Access to Host Certificate Store {#flatpak_access_to_host_certificate_store}

``` bash
flatpak override --user \
  --filesystem=host-etc:ro \
  --env=SSL_CERT_FILE=/run/host/etc/pki/ca-trust/extracted/pem/tls-ca-bundle.pem \
  com.devolutions.remotedesktopmanager
```

#### Reason

**Problem**: Flatpak apps are sandboxed and don't automatically trust
custom CA certs installed on the host (`/etc/pki/ca-trust`). This causes
SSL errors when connecting to services using internal/self-signed certs.

- `host-etc:ro`: gives **read-only access** to host's `/etc` inside
  sandbox.

<!-- -->

- `SSL_CERT_FILE=...`: tells OpenSSL inside the Flatpak to use **host's
  trusted CA bundle**, which includes your custom certs.

**Why this is best practice**:

- **No modification of the immutable OS** (Fedora Kinoite stays clean).
- **Persists across updates** (applies via Flatpak's override system).
- **Minimal and secure**: only read-only access to certs, limited to the
  app that needs it.
- Avoids bundling certs inside Flatpaks or using insecure workarounds.
- Works cleanly with standard Flatpak features.

This is a secure, update-proof way to let Flatpak apps trust your
internal certs---sandbox integrity stays intact.

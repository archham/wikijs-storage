## Introduction

## Standard Setup Proxmox DCM 1.0 {#standard_setup_proxmox_dcm_1.0}

**/etc/apt/sources.list.d/pdm-enterprise.sources**

``` bash
# Types: deb
# URIs: https://enterprise.proxmox.com/debian/pdm
# Suites: trixie
# Components: pdm-enterprise
# Signed-By: /usr/share/keyrings/proxmox-archive-keyring.gpg
```

**/etc/apt/sources.list.d/proxmox.sources**

``` bash
Types: deb
URIs: http://download.proxmox.com/debian/pdm
Suites: trixie
Components: pdm-no-subscription
Signed-By: /usr/share/keyrings/proxmox-archive-keyring.gpg
```

## Standard Setup Proxmox 9.0 {#standard_setup_proxmox_9.0}

/etc/apt/sources.list.d/pve-enterprise.sources

`vi /etc/apt/sources.list.d/pve-enterprise.sources`\
\
`# Types: deb`\
`# URIs: https://enterprise.proxmox.com/debian/pve`\
`# Suites: trixie`\
`# Components: pve-enterprise`\
`# Signed-By: /usr/share/keyrings/proxmox-archive-keyring.gpg`

/etc/apt/sources.list.d/proxmox.sources

`vi /etc/apt/sources.list.d/proxmox.sources`\
\
`Types: deb`\
`URIs: http://download.proxmox.com/debian/pve`\
`Suites: trixie`\
`Components: pve-no-subscription`\
`Signed-By: /usr/share/keyrings/proxmox-archive-keyring.gpg`

/etc/apt/sources.list.d/ceph.sources

`vi /etc/apt/sources.list.d/ceph.sources`\
`Types: deb`\
`URIs: `[`http://download.proxmox.com/debian/ceph-squid`](http://download.proxmox.com/debian/ceph-squid)\
`Suites: trixie`\
`Components: no-subscription`\
`Signed-By: /usr/share/keyrings/proxmox-archive-keyring.gpg`

## Standard Setup Proxmox 8.0 {#standard_setup_proxmox_8.0}

/etc/apt/sources.list

`vi /etc/apt/sources.list`\
\
`deb `[`http://ftp.debian.org/debian`](http://ftp.debian.org/debian)` bookworm main contrib`\
\
`deb `[`http://ftp.debian.org/debian`](http://ftp.debian.org/debian)` bookworm-updates main contrib`\
\
`# security updates`\
`deb `[`http://security.debian.org/debian-security`](http://security.debian.org/debian-security)` bookworm-security main contrib`\
\
`# not for production use`\
`deb `[`http://download.proxmox.com/debian/pve`](http://download.proxmox.com/debian/pve)` bookworm pve-no-subscription`

/etc/apt/sources.list.d/pve-enterprise.list

`vi /etc/apt/sources.list.d/pve-enterprise.list`\
\
`# deb `[`https://enterprise.proxmox.com/debian/pve`](https://enterprise.proxmox.com/debian/pve)` bookworm pve-enterprise`

/etc/apt/sources.list.d/ceph.list

`vi /etc/apt/sources.list.d/ceph.list`\
\
`deb `[`http://download.proxmox.com/debian/ceph-quincy`](http://download.proxmox.com/debian/ceph-quincy)` bookworm no-subscription`\
`# deb `[`https://enterprise.proxmox.com/debian/ceph-quincy`](https://enterprise.proxmox.com/debian/ceph-quincy)` bookworm enterprise`

# GPU Passthrough {#gpu_passthrough}

TO BE CORRECTED

Gute Anleitung
<https://3os.org/infrastructure/proxmox/gpu-passthrough/igpu-passthrough-to-vm/#linux-virtual-machine-igpu-passthrough-configuration>

;

`vi /etc/default/grub`\
`...`\
`# If you change this file, run 'update-grub' afterwards to update`\
`# /boot/grub/grub.cfg.`\
`# For full documentation of the options in this file, see:`\
`#   info -f grub -n 'Simple configuration'`\
\
`GRUB_DEFAULT=0`\
`GRUB_TIMEOUT=5`\
`` GRUB_DISTRIBUTOR=`lsb_release -i -s 2> /dev/null || echo Debian` ``\
`<b>`{=html}`#GRUB_CMDLINE_LINUX_DEFAULT="quiet"``</b>`{=html}\
`<b>`{=html}`GRUB_CMDLINE_LINUX_DEFAULT="quiet amd_iommu=on iommu=pt"``</b>`{=html}\
`GRUB_CMDLINE_LINUX=""`\
`...`\
\
`update-grub`\
`reboot now`

;

`#vi /etc/modules   --> Only if needed for physical use of graphics card for VM (Display out)`\
`update-initramfs -u -k all`\
`echo "blacklist radeon" >> /etc/modprobe.d/blacklist.conf`\
`echo "blacklist nouveau" >> /etc/modprobe.d/blacklist.conf`\
`echo "blacklist nvidia" >> /etc/modprobe.d/blacklist.conf`\
`reboot now`

;

`pvesh get /nodes/pve/hardware/pci --pci-class-blacklist ""`\
`#Now find the Device you need, in my case 07:00 `\
`# Output:`\
`...`\
`│ 0x030000 │ 0x13c0 │ ``<b>`{=html}`0000:07:00.0``</b>`{=html}` │         18 │ 0x10de │ GM204 [GeForce GTX 980] `\
`...`\
`lspci -n -s 07:00`\

DUMP

;

`   1  vi /etc/apt/sources.list`\
`   2  vi /etc/apt/sources.list`\
`   3  vi /etc/apt/sources.list.d/pve-enterprise.list`\
`   4  apt dist-upgrade `\
`   5  apt update`\
`   6  apt-get update `\
`   7  vi /etc/apt/sources.list.d/pve-enterprise.list`\
`   8  vi /etc/apt/sources.list`\
`   9  apt update`\
`  10  cd /etc/`\
`  11  grep -v `[`https://enterprise.proxmox.com/debian/ceph-quincy`](https://enterprise.proxmox.com/debian/ceph-quincy)` bookworm InRelease`\
`  12  grep -f `[`https://enterprise.proxmox.com/debian/ceph-quincy`](https://enterprise.proxmox.com/debian/ceph-quincy)` bookworm InRelease`\
`  13  grep -i .  `[`https://enterprise.proxmox.com/debian/ceph-quincy`](https://enterprise.proxmox.com/debian/ceph-quincy)` bookworm InRelease`\
`  14  cd`\
`  15  grep -i `[`https://enterprise.proxmox.com/debian/ceph-quincy`](https://enterprise.proxmox.com/debian/ceph-quincy)` bookworm InRelease /etc/`\
`  16  grep -i "`[`https://enterprise.proxmox.com/debian/ceph-quincy`](https://enterprise.proxmox.com/debian/ceph-quincy)` bookworm InRelease" /etc/`\
`  17  grep -r "`[`https://enterprise.proxmox.com/debian/ceph-quincy`](https://enterprise.proxmox.com/debian/ceph-quincy)` bookworm InRelease" /etc/`\
`  18  grep -r "`[`https://enterprise.proxmox.com/debian/ceph-quincy`](https://enterprise.proxmox.com/debian/ceph-quincy)` bookworm InRelease" /etc/`\
`  19  grep -r "`[`https://enterprise.proxmox.com/debian/ceph-quincy`](https://enterprise.proxmox.com/debian/ceph-quincy)` bookworm InRelease" /etc/`\
`  20  grep -r "`[`https://enterprise.proxmox.com/debian/ceph-quincy`](https://enterprise.proxmox.com/debian/ceph-quincy)` bookworm InRelease" /etc/`\
`  21  grep -r "`[`https://enterprise.proxmox.com/debian/ceph-quincy`](https://enterprise.proxmox.com/debian/ceph-quincy)` bookworm InRelease"`\
`  22  grep -r "`[`https://enterprise.proxmox.com/debian/`](https://enterprise.proxmox.com/debian/)`"`\
`  23  grep -w "`[`https://enterprise.proxmox.com/debian/ceph-quincy`](https://enterprise.proxmox.com/debian/ceph-quincy)` bookworm InRelease"`\
`  24  grep -rw "`[`https://enterprise.proxmox.com/debian/ceph-quincy`](https://enterprise.proxmox.com/debian/ceph-quincy)` bookworm InRelease" /etc/`\
`  25  grep -rw "`[`https://enterprise.proxmox.com/debian/ceph-quincy`](https://enterprise.proxmox.com/debian/ceph-quincy)` bookworm InRelease" /etc/`\
`  26  grep -rw "`[`https://enterprise.proxmox.com/debian/ceph-quincy`](https://enterprise.proxmox.com/debian/ceph-quincy)` bookworm InRelease"`\
`  27  grep -w "`[`https://enterprise.proxmox.com/debian/ceph-quincy`](https://enterprise.proxmox.com/debian/ceph-quincy)` bookworm InRelease"`\
`  28  grep -rd "`[`https://enterprise.proxmox.com/debian/ceph-quincy`](https://enterprise.proxmox.com/debian/ceph-quincy)` bookworm InRelease"`\
`  29  grep -d "`[`https://enterprise.proxmox.com/debian/ceph-quincy`](https://enterprise.proxmox.com/debian/ceph-quincy)` bookworm InRelease"`\
`  30  man grep`\
`  31  grep -f "`[`https://enterprise.proxmox.com/debian/ceph-quincy`](https://enterprise.proxmox.com/debian/ceph-quincy)` bookworm InRelease"`\
`  32  man grep`\
`  33  grep -r '`[`https://enterprise.proxmox.com/debian/ceph-quincy`](https://enterprise.proxmox.com/debian/ceph-quincy)` bookworm InRelease' /etc/`\
`  34  grep -r '`[`https://enterprise.proxmox.com/debian/ceph-quincy`](https://enterprise.proxmox.com/debian/ceph-quincy)` bookworm InRelease' /`\
`  35  vi /etc/apt/sources.list.d/ceph.list`\
`  36  vi /etc/apt/sources.list.d/ceph.list`\
`  37  apt update `\
`  38  apt upgrade`\
`  39  apt update `\
`  40  apt upgrade`\
`  41  reboot noe`\
`  42  vi /etc/default/grub`\
`  43  vi /etc/default/grub`\
`  44  update-grub`\
`  45  reboot now`\
`  46  vi /etc/modules`\
`  47  update-initramfs -u -k all`\
`  48  echo "blacklist radeon" >> /etc/modprobe.d/blacklist.conf`\
`  49  echo "blacklist nouveau" >> /etc/modprobe.d/blacklist.conf`\
`  50  echo "blacklist nvidia" >> /etc/modprobe.d/blacklist.conf`\
`  51  cat /etc/modprobe.d/blacklist.conf `\
`  52  reboot now`\
`  53  dmesg | grep -e DMAR -e IOMMU -e AMD-Vi`\
`  54   find /sys/kernel/iommu_groups/ -type l`\
`  55  lspci -nnk`\
`  56  lspci -nnk | grep vfio`\
`  57  lspci -n`\
`  58  lspci -nnk | grep "Kernel driver in use"`\
`  59  dmesg | grep -e DMAR -e IOMMU -e AMD-Vi`\
`  60  vi /etc/modprobe.d/pve-blacklist.conf `\
`  61  vi /etc/modprobe.d/blacklist.conf `\
`  62  dmesg | grep "IOMMU"`\
`  63  find /sys/kernel/iommu_groups/ -type l`\
`  64  lspci -v`\
`  65  lspci -n -s 08:00`\
`  66  history`\
`  67  vi /etc/modules`\
`  68  update-initramfs -u -k all`\
`  69  reboot now`\
`  70  reboot now`\
`  71  ls`\
`  72  ll`\
`  73  uptime`\
`  74  history `\
`  75  dmesg | grep -e DMAR -e IOMMU -e AMD-Vi`\
`  76  find /sys/kernel/iommu_groups/ -type l`\
`  77  lspci -nnk`\
`  78  lspci -nnk | grep "Kernel driver in use"`\
`  79  echo "options vfio-pci ids=10de:13c0,10de:0fbb disable_vga=1"> /etc/modprobe.d/vifo.conf`\
`  80  update-initramfs -u`\
`  81  reset`\
`  82  reboot now`\
`  83  qm agent 100 ping`\
`  84  qm agent 100 ping`\
`  85  tailf -f /var/log/*`\
`  86  tail -f /var/log/pveam.log `\
`  87  tail -f /var/log/*`\
`  88  tail -f /var/log/pveam.log `\
`  89  lspci -nnk`\
`  90  lsmod | grep vfio`\
`  91  modprobe`\
`  92  lspci -nnk -s 80:00.0`\
`  93  lspci -nnk -s 08:00.0`\
`  94  history `\
`  95  sudo reboot now`\
`  96  qm agent 102 ping`\
`  97  qm agent 102 ping`\
`  98  qm agent 102 ping`\
`  99  qm agent 102 ping`\
` 100  qm agent 102 ping`\
` 101  qm agent 102 ping`\
` 102  qm agent 100 ping`\
` 103  qm agent 102 ping`\
` 104  qm agent 102 ping`\
` 105  qm agent 102 ping`\
` 106  qm agent 102 ping`\
` 107  qm agent 102 ping`\
` 108  qm agent 102 ping`\
` 109  qm agent 102 ping`\
` 110  qm agent 102 ping`\
` 111  uptime `\
` 112  cat /etc/os-release `\
` 113  curl -so wazuh-agent.deb `[`https://packages.wazuh.com/4.x/apt/pool/main/w/wazuh-agent/wazuh-agent_4.4.5-1_amd64.deb`](https://packages.wazuh.com/4.x/apt/pool/main/w/wazuh-agent/wazuh-agent_4.4.5-1_amd64.deb)` && sudo WAZUH_MANAGER='192.168.0.136' WAZUH_AGENT_GROUP='default' WAZUH_AGENT_NAME='svproxmox01p' dpkg -i ./wazuh-agent.deb`\
` 114  curl -so wazuh-agent.deb `[`https://packages.wazuh.com/4.x/apt/pool/main/w/wazuh-agent/wazuh-agent_4.4.5-1_amd64.deb`](https://packages.wazuh.com/4.x/apt/pool/main/w/wazuh-agent/wazuh-agent_4.4.5-1_amd64.deb)` && WAZUH_MANAGER='192.168.0.136' WAZUH_AGENT_GROUP='default' WAZUH_AGENT_NAME='svproxmox01p' dpkg -i ./wazuh-agent.deb`\
` 115  apt-get dist-upgrade `\
` 116  apt --fix-broken install`\
` 117  apt-get dist-upgrade `\
` 118  htop`\
` 119  top`\
` 120  cat /var/log/faillog `\
` 121  cat /var/log/lastlog `\
` 122  cat /var/log/*`\
` 123  cat /var/log/journal/be679d46f43445338a1c8119bd41068b/system.journal `\
` 124  qm unlock 105`\
` 125  qm unlock 105`\
` 126  history`

[index.php?title=Category:Hypervisor](index.php?title=Category:Hypervisor "index.php?title=Category:Hypervisor"){.wikilink}
[index.php?title=Category:Linux](index.php?title=Category:Linux "index.php?title=Category:Linux"){.wikilink}
[index.php?title=Category:Hypervisor](index.php?title=Category:Hypervisor "index.php?title=Category:Hypervisor"){.wikilink}
[index.php?title=Category:Linux](index.php?title=Category:Linux "index.php?title=Category:Linux"){.wikilink}

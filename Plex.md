# Introduction

DUMP 11.10.23

;

`   1  ll`\
`   2  vi /etc/yum.repos.d/plex.repo`\
`   3  sudo dnf install epel-release`\
`   4  sudo dnf config-manager --add-repo `[`https://developer.download.nvidia.com/compute/cuda/repos/rhel9/x86_64/cuda-rhel9.repo`](https://developer.download.nvidia.com/compute/cuda/repos/rhel9/x86_64/cuda-rhel9.repo)\
`   5  sudo dnf install kernel-devel-$(uname -r) kernel-headers-$(uname -r)`\
`   6  sudo dnf install nvidia-driver nvidia-settings`\
`   7  sudo dnf install cuda-driver`\
`   8  sudo systemctl daemon-reload `\
`   9  nvidia-smi`\
`  10  sudo reboot now`\
`  11  nvidia-smi`\
`  12  dnf update`\
`  13  update-crypto-policies --set DEFAULT:SHA1`\
`  14  dnf install --nogpgcheck plexmediaserver`\
`  15  systemctl status plexmediaserver.service `\
`  16  sudo firewall-cmd --add-port=32400/tcp --permanent`\
`  17  sudo firewall-cmd --reload`\
`  18  vi /etc/fstab`\
`  19  vi smb.cred`\
`  20  mkdir /media/Plex`\
`  21  dnf install cifs-utils`\
`  22  chmod 600 ~/smb.cred`\
`  23  systemctl daemon-reload `\
`  24  mount -a`\
`  25  nvidia-smi`

# Install on Linux {#install_on_linux}

;

`vi /etc/fstab`\
\
`#`\
`# /etc/fstab`\
`# Created by anaconda on Fri Jul  7 13:32:38 2023`\
`#`\
`# Accessible filesystems, by reference, are maintained under '/dev/disk/'.`\
`# See man pages fstab(5), findfs(8), mount(8) and/or blkid(8) for more info.`\
`#`\
`# After editing this file, run 'systemctl daemon-reload' to update systemd`\
`# units generated from this file.`\
`#`\
`/dev/mapper/rl-root     /                       xfs     defaults        0 0`\
`UUID=8dfd2d86-1d1a-43fd-89fe-888d8e3fc1b0 /boot                   xfs     defaults        0 0`\
`/dev/mapper/rl-swap     none                    swap    defaults        0 0`\
`//IP_ADDRESS/video /media/Plex  cifs  credentials=/root/smb.cred  0 0`

------------------------------------------------------------------------

`vi smb.cred`\
\
`username=USERNAME`\
`password=***`

------------------------------------------------------------------------

`vi /etc/yum.repos.d/plex.repo`\
\
`[Plex]`\
`name=Plex`\
`baseurl=`[`https://downloads.plex.tv/repo/rpm/$basearch/`](https://downloads.plex.tv/repo/rpm/$basearch/)\
`enabled=1`\
`gpgkey=`[`https://downloads.plex.tv/plex-keys/PlexSign.key`](https://downloads.plex.tv/plex-keys/PlexSign.key)\
`gpgcheck=1`

------------------------------------------------------------------------

`   1  lsblk `\
`   2  sudo dnf install epel-release`\
`   3  sudo dnf config-manager --add-repo `[`https://developer.download.nvidia.com/compute/cuda/repos/rhel9/x86_64/cuda-rhel9.repo`](https://developer.download.nvidia.com/compute/cuda/repos/rhel9/x86_64/cuda-rhel9.repo)\
`   4  sudo dnf install kernel-devel-$(uname -r) kernel-headers-$(uname -r)`\
`   5  sudo dnf install nvidia-driver nvidia-settings`\
`   6  sudo dnf install cuda-driver`\
`   7  nvidia-smi`\
`   8  sudo systemctl daemon-reload `\
`   9  nvidia-smi`\
`  10  sudo reboot now`\
`  11  tail -f /var/log/messages `\
`  12  sudo su -`\
`  13  history `\
`  14  sudo su -`\
`  15  sudo shutdown now`\
`  16  history `

------------------------------------------------------------------------

`   1  tail -f /var/log/messages `\
`   2  ip a`\
`   3  history `\
`   4  exit`\
`   5  history `\
`   6  nvidia-smi `\
`   7  ll`\
`   8  pwd`\
`   9  vi x`\
`  10  shutdown now`\
`  11  nvidia-smi `\
`  12  ll`\
`  13  yum install qemu-guest-agent`\
`  14  systemctl status qemu-guest-agent`\
`  15  hostname`\
`  16  nvidia-smi `\
`  17  sudo reboot now`\
`  18  nvidia-smi `\
`  19  vi /etc/yum.repos.d/plex.repo`\
`  20  dnf install plexmediaserver -y`\
`  21  update-crypto-policies --set DEFAULT:SHA1`\
`  22  dnf install --nogpgcheck plexmediaserver`\
`  23  update-crypto-policies --set DEFAULT:NO-SHA1`\
`  24  systemctl status plexmediaserver.service `\
`  25  sudo firewall-cmd --add-port=32400/tcp --permanent`\
`  26  sudo firewall-cmd --reload`\
`  27  vi /etc/fstab `\
`  28  ss -taube`\
`  29  vi smb.cred`\
`  30  ll`\
`  31  mkdir /media/Plex`\
`  32  dnf install cifs-utils`\
`  33  dnf install nfs-utils`\
`  34  chmod 600 ~/smb.cred`\
`  35  vi /etc/fstab`\
`  36  mount -a`\
`  37  systemctl daemon-reload `\
`  38  mount -a`\
`  39  ip a`\
`  40  hostname`\
`  41  reboot now`\
`  42  nvidia-smi `\
`  43  hostnamectl `\
`  44  hostnamectl set-hostname svplex01p.local`\
`  45  hostnamectl `\
`  46  nmtui`\
`  47  reboot now`\
`  48  htop`\
`  49  top`\
`  50  dnf install htop`\
`  51  htop`\
`  52  nvidia-smi `\
`  53  htop`\
`  54  nvidia-smi `\
`  55  nvidia-settings `\
`  56  nvidia-smi `\
`  57  htop`\
`  58  nvidia-smi `\
`  59  htop`\
`  60  nvidia-smi `\
`  61  htop`\
`  62  nvidia-smi `\
`  63  htop`\
`  64  nvidia-smi `\
`  65  htop`\
`  66  nvidia-smi `\
`  67  htop`\
`  68  nvidia-smi `\
`  69  reboot now`\
`  70  htop`\
`  71  nvidia-smi `\
`  72  reboot now`\
`  73  htop`\
`  74  nvidia-smi `\
`  75  htop`\
`  76  nvidia-smi `\
`  77  htop`\
`  78  nvidia-smi `\
`  79  htop`\
`  80  nvidia-smi `\
`  81  shutdown now`\
`  82  htop`\
`  83  nvidia-smi `\
`  84  htop`\
`  85  shutdown now`\
`  86  htop`\
`  87  nvidia-smi `\
`  88  htop`\
`  89  nvidia-smi `\
`  90  htop`\
`  91  nvidia-smi `\
`  92  lsblk `\
`  93  nvidia-smi `\
`  94  lsblk `\
`  95  nvidia-smi `\
`  96  htop`\
`  97  nvidia-smi `\
`  98  ll`\
`  99  htop`\
` 100  dnf update`\
` 101  nvidia-smi `\
` 102  exit`\
` 103  history | grep apt`\
` 104  history | grep dnf`\
` 105  history | grep yum`\
` 106  hostname`\
` 107  exit`\
` 108  cat /etc/fstab `\
` 109  vi /etc/fstab `\
` 110  sudo WAZUH_MANAGER='192.168.0.136' WAZUH_AGENT_GROUP='default' WAZUH_AGENT_NAME='svplex01p' yum install -y `[`https://packages.wazuh.com/4.x/yum/wazuh-agent-4.4.5-1.x86_64.rpm`](https://packages.wazuh.com/4.x/yum/wazuh-agent-4.4.5-1.x86_64.rpm)\
` 111  sudo systemctl daemon-reload`\
` 112  sudo systemctl enable wazuh-agent`\
` 113  sudo systemctl start wazuh-agent`\
` 114  sudo dnf install chkconfig -y`\
` 115  sudo systemctl enable wazuh-agent`\
` 116  grep -rf "syscheck" /`\
` 117  grep -rf / "syscheck" `\
` 118  grep -rfw / "syscheck" `\
` 119  man grep`\
` 120  grep -elrf / "syscheck" `\
` 121  grep -el / "syscheck" `\
` 122  grep -el /* "syscheck" `\
` 123  exit`\
` 124  history`

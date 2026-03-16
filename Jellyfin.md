## Introduction

Jellyfin is a usefull tool to organize your media.

## Setup with Ubuntu 24.04 {#setup_with_ubuntu_24.04}

This is a tutorial to setup Jellyfin on Ubuntu 24.04 LTS

### Mount SMB Share {#mount_smb_share}

``` bash
#Get root rights
sudo su -

#Install cifs-utils
apt install cifs-utils

#Create mount point
mkdir /media/jellyfin

#Create mount credentials
touch smb.cred && chmod 600 smb.cred && vi smb.cred
-------------
#Insert mount credentials
username=<SMB share user>
password=<smb share pw>
-------------

#Add mount config
vi /etc/fstab 
-------------
#Insert mount config
//<SMB share ip>/<Share directory> /media/jellyfin cifs credentials=/root/smb.cred 0 0
-------------

#Reload systemctl daemon
systemctl daemon-reload 

#Mount share
mount -a

#Check share
ll /media/jellyfin/

#Reboot for good mesure
reboot now
```

### Install Jellyfin {#install_jellyfin}

``` bash
#Source https://jellyfin.org/docs/general/installation/linux/
curl https://repo.jellyfin.org/install-debuntu.sh | sudo bash
```

## Setup application {#setup_application}

Naming of Shows:

<https://jellyfin.org/docs/general/server/media/shows>

[Category:Linux](Category:Linux "Category:Linux"){.wikilink}

## General

General support for Linux topics.

## Explainshell - Shell commands explained {#explainshell___shell_commands_explained}

<https://explainshell.com/>

## Desktop Environments for Linux {#desktop_environments_for_linux}

In Linux, there are so many choices, and this includes the desktop
environments and window managers. The most popular desktop environments
in Linux are GNOME, Unity, Cinnamon, MATE, KDE, Xfce, and LXDE. All of
them offer sophisticated point-and-click graphical user interfaces (GUI)
which are on par with the desktop environments found in Windows and Mac
OS X. \...One great feature of Linux is that programs / applications
that are made to run in any one of these desktop environments will
normally work in the others. For example, GNOME Games can also run in
KDE, Xfce, or LXDE while KDE Games can likewise run in GNOME, Xfce, and
LXDE \... \... However, when installing multiple Linux desktop
environments / window managers on the same computer, it is important to
know that there will be may redundancies between similar applications
(system tools, games, etc.) found within each \...
<https://renewablepcs.wordpress.com/about-linux/kde-gnome-or-xfce/>

## Adding Root and Intermediate Certificates {#adding_root_and_intermediate_certificates}

How to add Root and Intermediate Certificates on Debian and Fedora
Systems.

<https://thomas-leister.de/en/how-to-import-ca-root-certificate/>
<https://community.brave.com/t/https-ssl-certificate/458526>

UNDER CONSTRUCTION!!!!!

### System cert store {#system_cert_store}

You need to have the Certificates in a .crt format in order to add them.

Install cert

`snap install nordpass`

### Firefox cert store {#firefox_cert_store}

Install Nordpass

`snap install nordpass`

### Brave cert store {#brave_cert_store}

Install Nordpass

`snap install nordpass`

## XRDP

Add RDP to Linux. In this case to a Rocky Linux 9 machine.

``` bash
dnf install epel-release
dnf install xrdp
systemctl start xrdp
systemctl enable xrdp
firewall-cmd --permanent --add-port=3389/tcp
firewall-cmd --reload
```

## RDP GUI {#rdp_gui}

For use of RDP Sessions with multiple monitors but not all are intended
for use.

<https://unix.stackexchange.com/questions/119880/make-freerdp-prompt-user-for-username-and-password>

Code

`#!/bin/bash`\
\
`# XFreeRDP RemoteApp W/ Prompt Script`\
`# Version 0.4`\
`# Description:`\
`# XFreeRDP Remote App Script utilizing Zenity to populate variables`\
\
`OUTPUT=$(zenity --forms --title="Connection Information" \`\
`    --text="Enter information about your Remote Connection" \`\
`    --separator="," \`\
`    --add-entry="Server (Default: pc-ai-130157)" \`\
`    --add-entry="Port (Default: 3389)" \`\
`    --add-entry="Domain (Optional)" \`\
`    --add-entry="Username" \`\
`    --add-password="Password" \`\
`    --add-entry="Remote App Name (Optional)" \`\
`    --add-entry="Monitors (Default: 1,2)" \`\
`    --forms-date-format="%Y-%m-%d")`\
`OUTPUT_RESULTS=$?`\
`if ((OUTPUT_RESULTS != 0)); then`\
`    echo "Something went wrong"`\
`    exit 1`\
`fi`\
\
`Blank=""`\
`Server=$(awk -F, '{print $1}' <<<$OUTPUT)`\
`if [ "$Server" = "$Blank" ]; then`\
`    Server="pc-ai-130157.gv.li"`\
`else`\
`    Port="$Server"`\
`fi`\
\
`Port=$(awk -F, '{print $2}' <<<$OUTPUT)`\
`if [ "$Port" = "$Blank" ]; then`\
`    Port="3389"`\
`else`\
`    Port="$Port"`\
`fi`\
\
`Domain=$(awk -F, '{print $3}' <<<$OUTPUT)`\
`Username=$(awk -F, '{print $4}' <<<$OUTPUT)`\
`Password=$(awk -F, '{print $5}' <<<$OUTPUT)`\
`App=$(awk -F, '{print $6}' <<<$OUTPUT)`\
`Monitors=$(awk -F, '{print $7}' <<<$OUTPUT)`\
\
`if [ "$App" = "$Blank" ]; then`\
`    App="$App"`\
`    Title="$Server"`\
`else`\
`    AppName="$App"`\
`    Title="$AppName on $Server"`\
`    App="/app:||$App"`\
`fi`\
\
`if [ "$Monitors" = "$Blank" ]; then`\
`    Monitors="1,2"`\
`fi`\
\
`#zenity --info --title="Information Return" --text="$Server $Port $Domain $Username $Password $App"`\
`xfreerdp /t:"$Title" /v:$Server:$Port /d:$Domain /multimon /monitors:$Monitors /drive:_home_ebno_Share,/home/ebno/Share `\
`/u:$Username /p:$Password $App /cert-ignore +clipboard`\
\
`Password=""`

## GTK Dialog {#gtk_dialog}

Gtk dialog boxes are a convenient way to prompt the user for a small
amount of input, such as displaying a message or asking a question.

<https://stackoverflow.com/questions/7035/how-to-show-a-gui-message-box-from-a-bash-script-in-linux>

Everyone mentions zenity, there seem to be many others. A mixed up but
interesting list is at <http://alternativeto.net/software/zenity/>

### zenity:

First, an example of zenity featuring text formatting markup, window
title, button label.

;

`zenity \`\
`--info \`\
`--text="``<span size=\"xx-large\">`{=html}`Time is $(date +%Hh%M).``</span>`{=html}`\n\nGet your ``<b>`{=html}`coffee``</b>`{=html}`." \`\
`--title="Coffee time" \`\
`--ok-label="Sip"`

### gxmessage:

;

`gxmessage "my text"`

### xmessage:

xmessage is very old so it is stable and probably available in all
distributions that use X (since it\'s distributed with X). It is
customizable through X resources, for those that have been using Linux
or Unix for long enough to know what it means (.Xdefaults, anyone ?).

;

`xmessage -buttons Ok:0,"Not sure":1,Cancel:2 -default Ok -nearmouse "Is xmessage enough for the job ?" -timeout 10`

### kdialog (KDE tool): {#kdialog_kde_tool}

;

`kdialog --error "Some error occurred"`

### YAD (Yet Another Dialog): {#yad_yet_another_dialog}

Yad is included in newer Ubuntu versions. There is also this PPA: YAD:
Zenity On Steroids \[Display Graphical Dialogs From Shell Scripts\] \~
Web Upd8: Ubuntu / Linux blog. Does not seem to auto-size dialogs.

;

`echo My text | yad \`\
`--text-info \`\
`--width=400 \`\
`--height=200`

An bigger example

;

`yad \`\
`--title="Desktop entry editor" \`\
`--text="Simple desktop entry editor" \`\
`--form \`\
`--field="Type:CB" \`\
`--field="Name" \`\
`--field="Generic name" \`\
`--field="Comment" \`\
`--field="Command:FL" \`\
`--field="Icon" \`\
`--field="In terminal:CHK" \`\
`--field="Startup notify:CHK" "Application" "Name" "Generic name" "This is the comment" "/usr/bin/yad" "yad" FALSE TRUE \`\
`--button="WebUpd8:2" \`\
`--button="gtk-ok:0" \`\
`--button="gtk-cancel:1"`

### Others not in Ubuntu standard repositories: {#others_not_in_ubuntu_standard_repositories}

\- shellgui - xdialog - gtkdialog

### Off-topic (for terminal): {#off_topic_for_terminal}

;

`whiptail --msgbox "my text" 10 20`\
`dialog --msgbox "my text" 10 20`

## Aliases

Aliases are a very handy tool to create a shortend version of a
frequently used command by the user.

Example

`vim ~/.bashrc`\
\
`#My custom aliases`\
`alias s='ssh -l$USERNAME'`\
\
`#Reload config for current session`\
`source ~/.bashrc`

## Misc

### xkill

Kill x11 Software when clicking on it.

### whoami \| who am i {#whoami_who_am_i}

;

`USER@PCNAME:~/home$ whoami`\
`USER`\
\
`USER@PCNAME:~/home$ sudo su -`\
\
`root@PCNAME:~# who am i`\
`USER  pts/3        2023-09-28 15:19`

### paste clipboard with keyboard {#paste_clipboard_with_keyboard}

Source: <https://www.bitbull.ch/wiki/index.php/Linux_Short_Reference#paste_clipboard_with_keyboard>

`this may be super useful for console connections into VMs`\
`sh -c 'sleep 0.5; xdotool type "$(xclip -o -selection clipboard)"'`

### port checker function [^1] {#port_checker_function}

``` bash
check_port() {
  if [[ $# -ne 2 ]]; then
    echo "Usage: check_port <HOST> <PORT>"
    echo "Example: check_port 127.0.0.1 22"
    return 1
  else
    local host=$1
    local port=$2
    (echo > "/dev/tcp/$host/$port") >/dev/null 2>&1 && echo "$host,$port,open" || echo "$host,$port,closed"
  fi
}
```

# GPU

Everything to know about all the different GPU CLI Tools for every
vendor.

<https://www.cyberciti.biz/open-source/command-line-hacks/linux-gpu-monitoring-and-diagnostic-commands/>

- gpustat
- intel_gpu_top
- nvidia-smi
- nvtop
- radeontop

# Software

General Software for Linux

## PSensor - Temperatursensor {#psensor___temperatursensor}

Temperatursensor for GUI

<https://linuxhint.com/check-cpu-temperature-linux/>

Install psensor

`sudo apt install psensor`

## ncdu - NCurses Disk Usage {#ncdu___ncurses_disk_usage}

Ncdu is a disk usage analyzer with an ncurses interface. It is designed
to find space hogs on a remote server where you don't have an entire
graphical setup available\...

<https://dev.yorhel.nl/ncdu>

Install ncdu

`sudo apt install ncdu`\
`cd to/desired/directory && ncdu`

## Citrix Workspace App {#citrix_workspace_app}

;

`## How to install Citrix Workspace App  on Linux Mint`\
\
`#1: Download & install DEB Package`\
\
[`https://www.citrix.com/downloads/workspace-app/linux/workspace-app-for-linux-latest.html`](https://www.citrix.com/downloads/workspace-app/linux/workspace-app-for-linux-latest.html)\
\
`Double klick on DEB Package`\
\
`#2: Add missing Root CA (CTX203362 - SSL Error 61)`\
\
`sudo cp /etc/ssl/certs/SwissSign_Gold_CA_-_G2.pem /opt/Citrix/ICAClient/keystore/cacerts/SwissSign_Gold_CA_-_G2.pem`\
\
`#3: Start using Citrix`\
\
`Enjoy :=)`

## FortiClient

How to install FortiClient on Linux Mint 22
[Source](https://unix.stackexchange.com/questions/635444/unmet-dependencies-libappindicator1)
& Ubuntu 24.04. LTS

``` bash
# Dependecies on Linux Mint 22
sudo su -
mkdir /opt/forti_vpn_dependecies
cd /opt/forti_vpn_dependecies
wget http://mirrors.kernel.org/ubuntu/pool/universe/liba/libappindicator/libappindicator1_12.10.1+20.10.20200706.1-0ubuntu1_amd64.deb
wget http://mirrors.kernel.org/ubuntu/pool/universe/libd/libdbusmenu/libdbusmenu-gtk4_16.04.1+18.10.20180917-0ubuntu8_amd64.deb
apt install  ./libappindicator1_12.10.1+20.10.20200706.1-0ubuntu1_amd64.deb ./libdbusmenu-gtk4_16.04.1+18.10.20180917-0ubuntu8_amd64.deb

# Dependecies on Ubuntu 24.04.2 LTS
sudo apt install libnss3-tools
```

``` bash
# Download FortiClient from https://www.fortinet.com/support/product-downloads

## Deb package
wget https://links.fortinet.com/forticlient/deb/vpnagent
## RPM package
wget https://links.fortinet.com/forticlient/rhel/vpnagent

sudo apt install ./forticlient_vpn_7.4.0.1636_amd64.deb 
# else
sudo dpkg -i forticlient_vpn_7.4.0.1636_amd64.deb
```

How to uninstall

``` bash
sudo apt remove forticlient
sudo apt autoremove
sudo apt remove libappindicator1
sudo apt remove libdbusmenu-gtk4
```

# Linux Mint {#linux_mint}

Hints and tips for Linux Mint

## Snap(d)

Hint: The file \'nosnap.pref\' has to be moved or removed in order to
install snap(d)

Install Snap(d)

`sudo rm /etc/apt/preferences.d/nosnap.pref`\
`sudo apt install snapd`

## Nordpass

How to install Nordpass on Linux Mint.

Install Nordpass

`snap install nordpass`

reboot or newly login

`logout`

Start Nordpass

`nordpass`

Give permission to Nordpass to manage PW

`snap connect nordpass:password-manager-service`

## Brave Browser {#brave_browser}

Source: <https://brave.com/linux/> as of 11.05.2023

How to install Brave browser on Linux Mint

`sudo apt install curl`

`sudo curl -fsSLo /usr/share/keyrings/brave-browser-archive-keyring.gpg `[`https://brave-browser-apt-release.s3.brave.com/brave-browser-archive-keyring.gpg`](https://brave-browser-apt-release.s3.brave.com/brave-browser-archive-keyring.gpg)

`echo "deb [signed-by=/usr/share/keyrings/brave-browser-archive-keyring.gpg] `[`https://brave-browser-apt-release.s3.brave.com/`](https://brave-browser-apt-release.s3.brave.com/)` stable main"|sudo tee /etc/apt/sources.list.d/brave-browser-release.list`

`sudo apt update`

`sudo apt install brave-browser`

### Fix for error code \'i386\' {#fix_for_error_code_i386}

Error code for \'doesn\'t support architecture \'i386\' after \'\'sudo
apt update

Error code

`N: Skipping acquire of configured file 'main/binary-i386/Packages' as repository '`[`https://brave-browser-apt-release.s3.brave.com`](https://brave-browser-apt-release.s3.brave.com)` stable InRelease' doesn't support architecture 'i386'`

Fix for error code

`echo "deb [signed-by=/usr/share/keyrings/brave-browser-archive-keyring.gpg arch=amd64] `[`https://brave-browser-apt-release.s3.brave.com/`](https://brave-browser-apt-release.s3.brave.com/)` stable main"|sudo tee /etc/apt/sources.list.d/brave-browser-release.list`

`deb [signed-by=/usr/share/keyrings/brave-browser-archive-keyring.gpg arch=amd64] `[`https://brave-browser-apt-release.s3.brave.com/`](https://brave-browser-apt-release.s3.brave.com/)` stable main`

[Media:Linux.png](Index.php?title=Media:Linux.png "Media:Linux.png"){.wikilink}
[index.php?title=Category:Linux](index.php?title=Category:Linux "index.php?title=Category:Linux"){.wikilink}

[^1]: bitbull.ch -
    <https://www.bitbull.ch/wiki/index.php?title=Linux_Short_Reference#port_checker_function>

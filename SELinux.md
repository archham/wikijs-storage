debug notes:

```` bash
# SELinux debug
* Created date: 20250604

# Current status

grep ^SELINUX= /etc/selinux/config
sed -i 's/^SELINUX=.*/SELINUX=permissive/' /etc/selinux/config
getenforce

## disable in bootloader if needed (hardcore mode)

bash```
grubby --update-kernel ALL --args selinux=0
grubby --update-kernel ALL --remove-args selinux
```

best practice to disable SELinux in the config file to for clarity to the user (/etc/selinux/config)

#

ps -f fax | less # see all curent processes
systemctl list-units -t service | grep -v 'static' | less # list all services
systemctl --failed # list all failed status

dnf history list --all # list all install


sealert -a /var/log/audit/audit.log
> /var/log/audit/audit.log
reboot


sealert -a /var/log/audit/audit.log | grep ^Hash:
sealert -a /var/log/audit/audit.log | less

restorecon -FRv /

# How to handle non system packages, installed/unpacked locally

rpm -qf /opt/* # know which file relates to what package
fgrep -lir /opt/wls-plugin /etc/ # from output before search in /etc for services that relate (or /usr/local)
fgrep -lir /opt/jdk /etc/

rpm -qf /opt/* # check if files corelate to a package
rpm -V httpd # check for changed files
rpm -q --configfiles httpd # check what files can be changed (config files)

dnf list installed | grep httpd # repo source of installed package


# fix SELinux violations

> /var/log/audit/audit.log
systemctl restart httpd
sealert -a /var/log/audit/audit.log | grep ^Hash:

find / -type d -name 'hsperfdata_root' 2>/dev/null
semanage fcontext -l | grep '/tmp' | grep perfd

semanage fcontext -l | grep '/tmp' > before
````

[Category:SELinux](Category:SELinux "Category:SELinux"){.wikilink}

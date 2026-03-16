## Introduction

Install Vaultwarden on standalone Rocky Linux 9 host with
[Ansible](Ansible "Ansible"){.wikilink} & Docker CE locally installed.

The idea here is that you use the application behind a Reverse Proxy
like HAProxy or NGNIX Proxy. That configuration is not mentioned here.
Additionally it would be a **good idea** to encrypt the traffic behind
the Reverse Proxy and the server. Otherwise the traffic is not encrypted
and you can not use the CLI tool from Bitwarden [Password Manager
CLI](https://bitwarden.com/help/cli/).

▶ **Installation carried out and tested 17.02.2024** ◀

## Prequesites and install {#prequesites_and_install}

``` bash
#Become root
sudo su -
```

### Remove & install needed software {#remove_install_needed_software}

- **Git** and **ansible-core** packages are only needed if you haven\'t
  already got a functioning Docker host
- **Podman** has to be removed in order for Docker CE to be installed

``` bash
dnf install git ansible-core
dnf remove podman
```

### Install [Ansible](Ansible "Ansible"){.wikilink} & Docker CE {#install_ansible_docker_ce}

- Roles and Playbooks from [Ansible](Ansible "Ansible"){.wikilink} are
  stored under /etc/

``` bash
mkdir /etc/ansible /etc/ansible/roles /etc/ansible/playbooks
cd /etc/ansible/roles
git clone https://github.com/haxorof/ansible-role-docker-ce.git docker_ce
cd /etc/ansible/playbooks
```

``` yaml
vi setup-docker-ce.yml

-------------START-------------

- hosts: localhost
  become: True
  vars:
    docker_daemon_envs:
      # do not use HTTP_PROXY since it is used by bitwarden internally, which leads to proxy requests for internal docker services
      HTTPS_PROXY: "{{ lookup('env', 'HTTP_PROXY') }}"
      NO_PROXY: "{{ lookup('env', 'NO_PROXY') }}"
    docker_compose: true
  roles:
    - docker_ce
    
-------------END-------------
```

``` bash
ansible-playbook setup-docker-ce.yml
```

### Install Vaultwarden {#install_vaultwarden}

#### Firewalld

``` bash
sed -i 's/FirewallBackend=.*/FirewallBackend=iptables/' /etc/firewalld/firewalld.conf
grep FirewallBackend /etc/firewalld/firewalld.conf

systemctl restart firewalld
systemctl enable firewalld
firewall-cmd --add-service=https --permanent
systemctl restart firewalld
```

#### User Vaultwarden {#user_vaultwarden}

Still needed? Under revision. **Skip for now.**

``` bash
adduser bitwarden
usermod -aG docker bitwarden

mkdir /opt/bitwarden
chmod -R 700 /opt/bitwarden
chown -R bitwarden:bitwarden /opt/bitwarden

cd /opt/
ll
cd bitwarden/
ll
cd ..
rm -r bitwarden
mkdir vaultwarden
ll
chmod -R 700 /opt/vaultwarden/
chown -R bitwarden:bitwarden /opt/vaultwarden/
cd /srv/
ll
ll /opt/
mkdir vaultwarden
cd vaultwarden/

su - bitwarden
```

#### Admin PW {#admin_pw}

``` bash
dnf config-manager --set-enabled crb
dnf install epel-release
dnf search argon2
dnf install argon2
ADMIN_PASSWORD=$(tr -dc A-Za-z0-9_=%.,: < /dev/urandom | head -c 24)
echo ADMIN_PASSWORD=$ADMIN_PASSWORD
#Write This down!!!! Otherwise your admin access is lost forever!!!

ADMIN_TOKEN="$(echo -n $ADMIN_PASSWORD | argon2 "$(openssl rand -base64 32)" -e -id -k 65540 -t 3 -p 4 | sed 's#\$#\$\$#g')"
echo ADMIN_TOKEN=$ADMIN_TOKEN
#Salt for Admin Token
```

#### Working directory {#working_directory}

``` bash
mkdir /srv/vaultwarden
cd /srv/vaultwarden
```

#### Docker Compose {#docker_compose}

``` yaml
vi docker-compose.yml

-------------START-------------

version: '3'

services:
  vaultwarden:
    image: vaultwarden/server:latest
    container_name: vaultwarden
    restart: always
    environment:
      ADMIN_TOKEN: "$$argon2id$$v=19$$m=65540,t=3,p=4$<SALT FOR ADMIN TOKEN>""
      DATABASE_URL: "mysql://vw:<MYSQL PW>@mariadb:3306/vw"
    volumes:
      - ./vw-data:/data
    ports:
      - "80:80"

  mariadb:
    image: mariadb:10.6
    environment:
      MYSQL_ROOT_PASSWORD: <MYSQL ROOT PW>
      MYSQL_DATABASE: vw
      MYSQL_USER: vw
      MYSQL_PASSWORD: <MYSQL PW>
    volumes:
      - ./mysql-data:/var/lib/mysql
    ports:
      - "3306:3306"

  mysql-cron-backup:
    image: fradelg/mysql-cron-backup
    depends_on:
      - mariadb
    volumes:
      - ./mariadb-backup:/backup
    environment:
      MYSQL_HOST: "mariadb"
      MYSQL_USER: "root"
      MYSQL_PASS: "<MYSQL ROOT PW>"
      MAX_BACKUPS: "15"
      INIT_BACKUP: "0"
      CRON_TIME: "1 1 * * *"
      GZIP_LEVEL: "9"
      MYSQLDUMP_OPTS: "--no-tablespaces"
    restart: "unless-stopped"

-------------END-------------
```

#### Vaultwarden Service {#vaultwarden_service}

``` bash
vi /etc/systemd/system/docker-vaultwarden.service

-------------START-------------

[Unit]
Description=Docker Compose: Vaultwarden

[Service]
Type=oneshot
WorkingDirectory=/srv/vaultwarden
ExecStart=/usr/bin/docker compose up -d
ExecStop=/usr/bin/docker compose down
RemainAfterExit=yes

[Install]
WantedBy=multi-user.target

-------------END-------------
```

``` bash
systemctl daemon-reload 
systemctl enable docker-vaultwarden.service 
systemctl start docker-vaultwarden.service 
systemctl status docker-vaultwarden.service 
```

### Testing

1.  Restart the service and check Webservice
2.  Restart the server and check Webservice
3.  Write data and do no. 1 or 2

``` bash
# Test for functionality
systemctl stop docker-vaultwarden.service 
systemctl start docker-vaultwarden.service 
systemctl status docker-vaultwarden.service 
```

#### Alias directory {#alias_directory}

``` bash
sudo su -

vi .bashrc
...
alias cda='cd /srv/vaultwarden'
...
```

### Update Vaultwarden {#update_vaultwarden}

``` bash
# Stop service
systemctl stop docker-vaultwarden

# cd and pull lates images
cda && docker compose pull
## or alternatively
cd /srv/vaultwarden && docker compose pull

# Start service and check status
systemctl start docker-vaultwarden
systemctl status docker-vaultwarden
```

## Configuration Webservice {#configuration_webservice}

``` bash
http://IpOfYourServer/admin

##General Settings
Domain URL: https://YourDesiredUrl.com

##Advanced settings
Client IP header: X-Forwarded-For

##SMTP Email Settings
Host: Mailserver
Secure SMTP: force_tls
Port: 465
From Address: Mailaddress@YourDesiredUrl.com
From Name: Vaultwarden
Username: Mailaddress@YourDesiredUrl.com
Password: xxxxxx
SMTP Auth mechanism: Login
SMTP connection timeout: 15
Server name sent during HELO: YourDesiredUrl.com
```

## Sources & Contributions {#sources_contributions}

[docker_ce](https://github.com/haxorof/ansible-role-docker-ce.git)

[Proxy-examples](https://github.com/dani-garcia/vaultwarden/wiki/Proxy-examples)

[Category:Docker](Category:Docker "Category:Docker"){.wikilink}
[Category:Linux](Category:Linux "Category:Linux"){.wikilink}
[Category:Ansible](Category:Ansible "Category:Ansible"){.wikilink}

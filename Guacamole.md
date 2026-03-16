## Guacamole

Install Apache Guacamole on standalone host with Ansible localy
installed.
[ansible.guacamole](https://github.com/joe-speedboat/ansible.guacamole)

### Prequesites and install {#prequesites_and_install}

Insert:

`curl -L ansible.bitbull.ch | bash`

`ansible-galaxy install joe-speedboat.guacamole`

`cd /etc/ansible/roles/joe-speedboat.guacamole/`\
`ansible-galaxy role install -r roles/requirements.yml`

`cd /etc/ansible/roles/joe-speedboat.mariadb/`\
`ansible-galaxy collection install -r collections/requirements.yml`

`ansible-galaxy collection install ansible.posix`

`ansible-galaxy collection install community.general`

`-----------`

`# MAKE FILE CHANGES`

`vi /etc/ansible/roles/joe-speedboat.guacamole/defaults/main.yml`\
`guacamole_version: 1.5.2`

`vi /etc/ansible/roles/joe-speedboat.guacamole/vars/main.yml`\
`guacamole_packages_EL:`\
`  - ca-certificates`\
`  - ghostscript`\
`  - java-11-openjdk`\
`  - "guacd.x86_64"`\
`  - "libguac.x86_64"`\
`  - "libguac-client-rdp.x86_64"`\
`  - "libguac-client-ssh.x86_64"`\
`  - "libguac-client-telnet.x86_64"`\
`  - "libguac-client-vnc.x86_64"`\
`  - dejavu-sans-mono-fonts`\
`  - freerdp`\
`  - tomcat`\
`  - mysql-connector-java`

`vi /etc/ansible/roles/joe-speedboat.guacamole/tests/install_guacamole.yml`\
`- hosts: localhost`

`-----------`

`cd /etc/ansible/roles/joe-speedboat.guacamole/`\
`ansible-playbook tests/install_guacamole.yml`

`systemctl status firewalld.service`

[Category:Ansible](Category:Ansible "Category:Ansible"){.wikilink}
[Category:Linux](Category:Linux "Category:Linux"){.wikilink}

## Installation

Installtion date 26.09.2023. The origins of the automated script come
from
[c-rieger.de](https://www.c-rieger.de/nextcloud-installation-mit-nur-einem-skript-zero-sh/).
Please consider contributing if you liked it.

### Preparation

- Install Ubuntu 22.04 LTS
- Install dependencies

;

`sudo -s`\
`apt install -y wget dnsutils`\
`wget -O zero.sh `[`https://codeberg.org/criegerde/nextcloud-zero/raw/branch/master/ubuntu.sh`](https://codeberg.org/criegerde/nextcloud-zero/raw/branch/master/ubuntu.sh)

- Make script executable

;

`chmod +x zero.sh`

- Edit necessary parameters if needed

;

`vi zero.sh`\
\
`...`\
\
`NEXTCLOUDDATAPATH="/data"`\
`NEXTCLOUDADMINUSER="nc_admin"`\
`NCRELEASE="latest.tar.bz2"`\
`PHPVERSION="8.2"`\
`LETSENCRYPT="n"`\
`NEXTCLOUDDNS="ihre.domain.de"`\
`DATABASE="m"`\
`NCDBUSER="ncdbuser"`\
`CURRENTTIMEZONE='Europe/Berlin'`\
`PHONEREGION='DE'`\
`NEXTCLOUDOFFICE="n"`\
`ONLYOFFICE="n"`\
`UPLOADSIZE='10G'`\
\
`...`

- Execute script

;

`./zero.sh`

## Configuration `<b>`{=html}Cloudflare & Let\'s Encrypt & HAProxy & Nextcloud`</b>`{=html} {#configuration_cloudflare_lets_encrypt_haproxy_nextcloud}

### Steps

`<b>`{=html}Cloudflare`</b>`{=html}

CNAME `<b>`{=html}SUBDOMAIN.YOUR.DOMAIN`</b>`{=html}

`<b>`{=html}Opnsense`</b>`{=html}

1\. ACME Client: Let\'s Encrypt Wildcard YOUR.DOMAIN

(Optional) Opnsense: Create virtual IP for HA Proxy (Entry point)

2\. HAProxy: Redirect to inernal Nextcloud Server

3\. Unbound DNS: Local DNS entry for reverse proxying to nextcloud

### PHP OPcache buffer {#php_opcache_buffer}

Error from Nextcloud *The OPcache buffer is nearly full. To assure that
all scripts can be hold in cache, it is recommended to apply
opcache.memory_consumption to your PHP configuration with a value higher
than 256.* [PHP
Documentation](https://www.php.net/manual/en/opcache.configuration.php#ini.opcache.memory-consumption)
[Nextcloud
Documentation](https://docs.nextcloud.com/server/27/admin_manual/installation/server_tuning.html#enable-php-opcache)

/etc/php/8.2/fpm/php.ini

`vi /etc/php/8.2/fpm/php.ini`\
\
`...`\
`; The OPcache shared memory storage size.`\
`opcache.memory_consumption=``<b>`{=html}`512``</b>`{=html}\
`...`\
\
`systemctl restart php8.2-fpm.service`

### Troubleshooting PHP-fpm {#troubleshooting_php_fpm}

- Good Guide
  [Source](https://www.c-rieger.de/nextcloud-und-php-troubleshooting/)
- Helps determine how to calculate the FPM parameters.[FPM
  Calculator](https://spot13.com/pmcalculator/)
  `<b>`{=html}ATTENTION!`</b>`{=html} please remind yourself, that you
  need to reserve memory for DB, Redis, OPcache and the System!
- [Nextcloud
  Docu](https://docs.nextcloud.com/server/stable/admin_manual/installation/server_tuning.html?highlight=fpm)

;

`vi /etc/php/8.2/fpm/pool.d/www.conf`\
\
`#Edit the following lines according to the previously mentioned FPM Calculator`\
`pm.max_children = 220`\
`pm.start_servers = 55`\
`pm.min_spare_servers = 55`\
`pm.max_spare_servers = 165`

### Troubleshooting PHP {#troubleshooting_php}

;

`; Maximum amount of memory a script may consume`\
`; `[`https://php.net/memory-limit`](https://php.net/memory-limit)\
`memory_limit = 14G`

### Adjust Upload File size {#adjust_upload_file_size}

[Nextcloud
Doku](https://docs.nextcloud.com/server/latest/admin_manual/configuration_files/big_file_upload_configuration.html)
[Nextcloud Client
Doku](https://docs.nextcloud.com/desktop/latest/advancedusage.html)

## Dropin Folder / ncput {#dropin_folder_ncput}

[Doku
Bitbull.ch](https://www.bitbull.ch/wiki/index.php/Linux_Short_Reference#nextcloud_upload_to_shared_link_with_curl)

;

`alias ncput='curl -u "otBzHiBQrTZrAtA:" -H "X-Requested-With: XMLHttpRequest" -X PUT "`[`https://cloud.rueti16.com/public.php/webdav/`](https://cloud.rueti16.com/public.php/webdav/)`" -T'`

## Maintenance Mode {#maintenance_mode}

Change into Nextcloud directory and enable/disable maintenance mode.

;

`# occ command is only available here`\
`cd /var/www/nextcloud`\
\
`# Enable maintenance mode`\
`sudo -u www-data php occ maintenance:mode --on`\
\
`# Disable maintenance mode`\
`sudo -u www-data php occ maintenance:mode --off`

<https://docs.nextcloud.com/server/latest/admin_manual/maintenance/backup.html#maintenance-mode>

## Anleitung

<https://www.c-rieger.de/nextcloud-installationsanleitung/>

<https://www.c-rieger.de/nextcloud-installation-mit-nur-einem-skript-zero-sh/>

## Migration

How to migrate to a new Nextcloud instance (no HA). Mostly needed
because of security reasons. See [Nextcloud
Migration](https://docs.nextcloud.com/server/latest/admin_manual/maintenance/migrating.html)
and [Nextcloud
Backup](https://docs.nextcloud.com/server/latest/admin_manual/maintenance/backup.html)
documentation.

### Steps {#steps_1}

1.  Set up ne Nextcloud instance
2.  Enable Maintenance mode on current Nextcloud instance
3.  Create a dump of the database and copy it to the new instance
4.  Copy all files with rsync

;

`rsync -Aavx /data_nvme/ root@192.168.3.110:/data/`

1.  Check the \"old\" system for a *data-fingerprint*
2.  Make some checks with a client that you changed the hosts file
3.  Release your new Nextcloud instance!

[Category:Linux](Category:Linux "Category:Linux"){.wikilink}

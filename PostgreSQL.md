# Installing PostgreSQL 16 on Rocky Linux 9 {#installing_postgresql_16_on_rocky_linux_9}

Installation tested 11/2025

Install instructions:
<https://docs.redhat.com/en/documentation/red_hat_enterprise_linux/9/html/configuring_and_using_database_servers/using-postgresql_configuring-and-using-database-servers>

# Datasheet:

- Rocky Linux 9
- SELinux - Enforcing
- CPU 8 cores
- RAM 8 GB
- PostgreSQL dirs
  - Working dir '/var/lib/pgsql/data'
  - Logs dir '/var/lib/pgsql/data/log'
- PostgreSQL admin user
  - dbadmin

# Install PostgreSQL 16 server and contrib package {#install_postgresql_16_server_and_contrib_package}

``` bash
sudo dnf module install postgresql:16/server -y
sudo dnf install postgresql-contrib -y
```

- Initialize the database

``` bash
sudo postgresql-setup --initdb
```

- Enable and start the service

``` bash
sudo systemctl enable --now postgresql
sudo systemctl status postgresql
```

## Basic Configuration and Hardening {#basic_configuration_and_hardening}

``` bash
sudo sed -i.bak \
  -e "s|^#\?listen_addresses.*|listen_addresses = '*'|" \
  -e "s|^#\?ssl *=.*|ssl = on|" \
  -e "s|^#\?ssl_cert_file.*|ssl_cert_file = '/var/lib/pgsql/data/server.crt'|" \
  -e "s|^#\?ssl_key_file.*|ssl_key_file = '/var/lib/pgsql/data/server.key'|" \
  -e "s|^#\?password_encryption.*|password_encryption = scram-sha-256|" \
  -e "s|^#\?logging_collector.*|logging_collector = on|" \
  -e "s|^#\?log_directory.*|log_directory = 'log'|" \
  -e "s|^#\?log_filename.*|log_filename = 'postgresql-%a.log'|" \
  -e "s|^#\?log_rotation_age.*|log_rotation_age = 1d|" \
  -e "s|^#\?log_min_duration_statement.*|log_min_duration_statement = 5000|" \
  /var/lib/pgsql/data/postgresql.conf
```

- SSL Certificate Setup (self-signed certificate)

``` bash
cd /var/lib/pgsql/data
```

- Generate private key

``` bash
sudo openssl genrsa -out server.key 2048
sudo chmod 600 server.key
sudo chown postgres:postgres server.key
```

- Generate self-signed certificate (valid for 10 years)

``` bash
sudo openssl req -new -x509 -days 3650 -key server.key -out server.crt \
  -subj "CN=yourserver.example.com"
sudo chown postgres:postgres server.crt
```

### Access Rules {#access_rules}

Comment out local connections with PAM

``` bash
sudo sed -i.bak '/^[[:space:]]*local\s\+all\s\+all\s\+peer$/ s/^/# /' /var/lib/pgsql/data/pg_hba.conf
```

Comment out IPv4 localhost connections with ident

``` bash
sudo sed -i.bak '/^[[:space:]]*host[[:space:]]\+all[[:space:]]\+all[[:space:]]\+127\.0\.0\.1\/32[[:space:]]\+ident$/ s/^/# /' /var/lib/pgsql/data/pg_hba.conf
```

Comment out IPv6 localhost connections with ident

``` bash
sudo sed -i.bak '/^[[:space:]]*host[[:space:]]\+all[[:space:]]\+all[[:space:]]\+::1\/128[[:space:]]\+ident$/ s/^/# /' /var/lib/pgsql/data/pg_hba.conf
```

add config

``` bash
sudo tee -a /var/lib/pgsql/data/pg_hba.conf <<EOF

### Added by $(logname) on $(date +%d.%m.%Y) ###

# Allow only secure local access
## Allow local access only with pw
local      all             all                                   scram-sha-256
## Allow IPv4 local connections only with pw
host       all             all             127.0.0.1/32          scram-sha-256
## Allow IPv6 local connections only with pw
host       all             all             ::1/128               scram-sha-256

# Allow only secure remote access from local subnets
## Admin net
hostssl    all             all             192.168.1.0/24        scram-sha-256
## Client net
hostssl    all             all             192.168.2.0/24        scram-sha-256
### END ###
EOF
```

### Firewall

Open PostgreSQL port

``` bash
sudo firewall-cmd --add-port=5432/tcp --permanent
sudo firewall-cmd --reload
sudo firewall-cmd --list-all
systemctl status firewalld.service
```

### PostgreSQL User Management {#postgresql_user_management}

Never use the postgres system user for interactive database work!

- Temporarily disable history (no PW in history) & add superuser dbadmin

``` bash
set +o history
sudo -u postgres psql -c "CREATE ROLE dbadmin WITH LOGIN SUPERUSER PASSWORD 'superstrongpassword';"
set -o history
```

``` bash
sudo systemctl restart postgresql
```

## Verification

``` bash
sudo systemctl status postgresql
```

## Tests

``` bash
psql -U dbadmin -d postgres -c "SHOW ssl;"
psql -U dbadmin -d postgres -c "SHOW password_encryption;"
psql -U dbadmin -d postgres -c "SELECT now(), inet_server_addr(), inet_server_port();"
```

- Login with 'dbadmin' and CREATE DATABASE 'test'

``` bash
psql -U dbadmin -h 127.0.0.1 -d postgres
```

``` sql
CREATE DATABASE test
    WITH
    OWNER = dbadmin
    ENCODING = 'UTF8'
    LOCALE_PROVIDER = 'libc'
    CONNECTION LIMIT = -1
    IS_TEMPLATE = False;
CREATE DATABASE
```

``` bash
\q
```

# Backup

To be configured. **DO NOT FORGET!**

# Restore

To be configured. **DO NOT FORGET!**

# Notes

Notes from RedHat \* The 'postgres' UNIX system user - should be used
only to run the PostgreSQL server and client applications, such as
'pg_dump'. Do not use the 'postgres' system user for any interactive
work on PostgreSQL administration, such as database creation and user
management.

Do not use 'postgres' user. Use 'dbadmin' with SUPERUSER role that was
created above!

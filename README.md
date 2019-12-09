# compose-gen.role
An Ansible role to deploy and control web server via docker-compose.
Supports:
- generic web service for php based sites (nginx/apache-php or php-fpm + percona/mysql/postgresql)
- NextCloud "personal cloud" storage (Main instance + additional instances)
- Plex Media Server (could use public storage with main instance of NextCloud)
- Transmission BT-client with web UI (could use public storage with main instance of NextCloud)
- TICK-stack based monitoring system
- Traefik load balancer with automated Let's Encrypt certificated issuing/renewing

Just add the role to your playbook and define the variables. You need to define only non-default vars.

# Common variables
compose_dir: "/opt/docker-compose"
base_dir: "/var/services"
storage_dir: "/var/data"
sites_dir: "/var/www"
docker_network: "net-webapps"
default_password: "password"
acme_email: "admin@example.com"
# Set to 'false' if you want run 'docker-compose up -d' by yourself
update_compose: true

#  Dependent common variables
cloudstorage_dir: "{{ sites_dir }}"
default_db_root_password: "{{ default_password }}"

# Traefik config
traefik_dashboard: true
traefik_dashboard_host: "traefik.example.com"
traefik_dashboard_auth_string: "user:$apr1$.NIX7JLw$niiVFwT3ZX1Af2MqVgaSt/"
traefik_ssl_enable: true
traefik_ssl_redirect: true

# Tick Stack monitoring config
monitoring_enabled: true
monitoring_sitename: "monitoring.example.com"
monitoring_dashboard_auth_string: "user:$apr1$.NIX7JLw$niiVFwT3ZX1Af2MqVgaSt/"
monitoring_influx_admin_user: "influx"
monitoring_influx_admin_password: "{{ default_password }}"
monitoring_influx_read_user: "chronograf"
monitoring_influx_read_password: "{{ default_password }}"
monitoring_influx_write_user: "telegraf"
monitoring_influx_write_password: "{{ default_password }}"
monitoring_ssl_enable: true
monitoring_ssl_redirect: true

# Main Nextcloud instance config (Transmission and Plex will work only with this instance)
nextcloud_enabled: false
nextcloud_sitename: "storage.example.com"
nextcloud_admin_login: "admin"
nextcloud_admin_password: "{{ default_password }}"
nextcloud_db_name: "nextcloud"
nextcloud_db_user: "nextcloud"
nextcloud_db_password: "{{ default_password }}"
nextcloud_ssl_enable: true
nextcloud_ssl_redirect: true
#nextcloud_common_storage:
#  - secret
#  - books

# Plex Mediaserver config
plex_enabled: false
plex_sitename: "plex.example.com"
plex_advertise_ip: "{{ ansible_default_ipv4.address }}"
plex_storage_dir: "{{ storage_dir }}/public"
plex_ssl_enable: true
plex_ssl_redirect: true

# Transmission bit-torrent client config
transmission_enabled: false
transmission_sitename: "loader.example.com"
transmission_dashboard_auth_string: "user:$apr1$.NIX7JLw$niiVFwT3ZX1Af2MqVgaSt/"
transmission_torrent_port: 54321
transmission_download_dir: "{{ storage_dir }}/public/downloads"
transmission_watch_dir: "{{ storage_dir }}/public/watch"
transmission_ssl_enable: true
transmission_ssl_redirect: true

# Examples of NFS v3 shares, websites and cloud storage (Nextcloud based) config
# Define them in host vars file.
#
# NFS v3 Server config
# nfs:
#  - share: "share"
#    acl:
#      - "{{ ansible_default_ipv4.address }}/32(rw,no_subtree_check,no_root_squash)"
#
# Websites config
# websites:
#  - sitename: "example.com"
#    ssl_enable: true
#    ssl_redirect: true
#    add_www_host: true
#    www_redirect: true
#    extended_security_headers: false
#    http_auth_enable: false
#    http_auth_string: "user:$apr1$.NIX7JLw$niiVFwT3ZX1Af2MqVgaSt/"
#    backend_type: "fpm"      # Could be "fpm", "apache"
#    backend_version: "7.3"   # php version (only for php-based backends)
#    db_type: "percona"       # Could be "percona", "mysql", "postgresql"
#    db_name: "{{ sitename-hyphened }}"
#    db_user: "{{ sitename-hyphened }}"
#    db_password: "{{ default_password }}"
#    db_root_password: "{{ default_password }}"
#
# Cloud storage (Nextcloud based) config
# cloudstorages:
#  - sitename: "storage.example.com"
#    ssl_enable: true
#    ssl_redirect: true
#    add_www_host: true
#    www_redirect: true
#    admin_login: "admin"
#    admin_password: {{ default_password }}
#    db_name: "nextcloud"
#    db_user: "nextcloud"
#    db_password: "{{ default_password }}"

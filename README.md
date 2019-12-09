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
For var list look in defaults/main.yml file.

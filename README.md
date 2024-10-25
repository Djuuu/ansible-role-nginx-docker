Ansible Role: Nginx-docker
==========================

Install Nginx Docker Compose project.

Based on LinuxServer.io image: https://docs.linuxserver.io/images/docker-nginx/

Includes Nginx ultimate bad bot blocker: https://github.com/mitchellkrogza/nginx-ultimate-bad-bot-blocker

Requirements
------------

Requires the following to be installed:
- docker
- docker compose

Role Variables
--------------

Common system variables:

```yaml
timezone: UTC
```

Common Docker projects variables:

```yaml
# Base directory for Docker projects
docker_projects_path: # /var/apps
```

Available role variables are listed below, along with default values (see `defaults/main.yml`):

```yaml
# Docker project variables

nginx_project_name: nginx

# Docker project dynamic vars (uses `docker_project_name` prefix, adapt if overriden)

# Traefik router entrypoints
nginx_traefik_entrypoints: 'http,https'
# Traefik router middlewares
nginx_traefik_middlewares:
  - "https-redirect@file"

# Main service additional docker-compose options (ex: cpu_shares, deploy, ...)
nginx_compose_service_additional_options: |
  #ports:
  #  - 80:80
  #  - 443:443


# Nginx project variables

# linuxserver/nginx image version
nginx_linuxserver_nginx_version: latest

nginx_app_volumes: []
#  - /home/user/myapp # (will be mounted in /app/myapp)
#  - src: /home/user/myotherapp
#    dest: /app/mythirdapp

# Additional external docker-compose networks
nginx_compose_additional_networks: []
#  - postgres_default
```

Optional target user variables:
```yaml
target_user:
target_group:
target_user_uid:
target_user_gid:
```
(when user/group information differ from Ansible user, _become_ should be used in playbook)

Additional config files
-----------------------

Additional files in the following location will be copied in the project's `config/` directory:

- `config/nginx/{{ inventory_hostname }}/*`

or if `target_user` is defined:

- `config/nginx/{{ inventory_hostname }}-{{ target_user }}/*`

See [linuxserver/nginx](https://docs.linuxserver.io/images/docker-nginx/) 
documentation for details about the base image.

Example file structure:

- `config/nginx/`
  - `some-hostname/`
    - `nginx/`
      - `site-confs/`
        - `myapp-vhost.conf`
        - `otherapp-vhost.conf`
      - `nginx.conf`
    - `php/`
      - `www2.conf`
    - `www/`
        - `index.html`  

  - `other-hostname-alice/`
      - `nginx/`
          - `site-confs/`
              - `alice-vhost.conf`
          - `nginx.conf`  

  - `other-hostname-bob/`
      - `nginx/`
          - `site-confs/`
              - `bob-vhost.conf`
          - `nginx.conf`  

Dependencies
------------

This role depends on :
- [djuuu.docker_project](https://github.com/Djuuu/ansible-role-docker-project)

Some variables allow integration with:
- [djuuu.traefik_docker](https://github.com/Djuuu/ansible-role-traefik-docker)

Example Playbook
----------------

```yaml
- hosts: all
  gather_facts: true
  gather_subset:
    - "!all"
    - "!min"
    - user_id

  roles:
    - djuuu.nginx_docker
```

License
-------

Beerware License

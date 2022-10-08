# Duckprom

Almost the same as [dockprom](https://github.com/stefanprodan/dockprom).

Key differences:
* https for grafana with [letsencrypt](https://letsencrypt.org/)
* logs with [lokki](https://grafana.com/oss/loki/)
* pushgateway in edge compose
* basic auth for metrics and logs in edge compose
* alertmanager removed
* [traefik](https://doc.traefik.io/traefik/) instead of [caddy](https://caddyserver.com/)

## Install

```bash
git clone https://github.com/nmix/duckprom.git
cd duckprom
# --- we need htpasswd for create basic auth credentials
sudo apt-get install apache2-utils
# --- create credentials string for user 'admin' and pass 'admin'
#     see details on https://doc.traefik.io/traefik/middlewares/http/basicauth/#configuration-examples
htpasswd -nb admin admin
# output will be something like this
# admin:$apr1$Y5JhEo6T$vgEnWCPsp1A3iXz9FWZGZ/

# --- run project
LETSENCRYPT_EMAIL=email-for-letsencrypt@example.com  \
  GRAFANA_HOST=grafana.example.com \
  BASIC_AUTH_CREDS='admin:$apr1$Y5JhEo6T$vgEnWCPsp1A3iXz9FWZGZ/' \
  docker-compose up -d
```

Go to https://grafana.example.com, default creds `admin`/`admin`. Change password after first login.

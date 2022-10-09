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
export BASIC_AUTH_CREDS=$(htpasswd -nb admin admin)
# --- create prometheus config
cp prometheus/prometheus.yaml.example prometheus/prometheus.yaml

# --- run project
LETSENCRYPT_EMAIL=email-for-letsencrypt@example.com  \
  GRAFANA_HOST=grafana.example.com \
  BASIC_AUTH_CREDS=$BASIC_AUTH_CREDS \
  docker-compose up -d
```

Go to https://grafana.example.com, default creds `admin`/`admin`. Change password after first login.

## Install Edge
```bash
wget docker-compose.edge.yaml
export BASIC_AUTH_CREDS=$(htpasswd -nb admin admin)
BASIC_AUTH_CREDS=$BASIC_AUTH_CREDS docker-compose -f docker-compose.edge.yaml up -d
```

## Nginx Exporter

Duckprom can scrape metrics from nginx logs with https://github.com/martin-helmich/prometheus-nginxlog-exporter.

Fix log format in nginx
```nginx
log_format upstream_time '$remote_addr - $remote_user [$time_local] '
                         '"$request" $status $body_bytes_sent '
                         '"$http_referer" "$http_user_agent"'
                         'rt=$request_time uct="$upstream_connect_time" uht="$upstream_header_time" urt="$upstream_response_time"';
access_log /var/log/nginx/access.log upstream_time;
```
Import [NGINX Log Metrics](https://grafana.com/grafana/dashboards/6482-nginx-log-metrics/) into duckprom grafana.

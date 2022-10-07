# Duckprom

Almost the same as [dockprom](https://github.com/stefanprodan/dockprom).

Key differences:
* https for grafana with [letsencrypt](https://letsencrypt.org/)
* logs with [lokki](https://grafana.com/oss/loki/)
* pushgateway in edge compose
* basic auth for metrics and logs in edge compose
* alertmanager removed
* [traefik](https://doc.traefik.io/traefik/) instead of [caddy](https://caddyserver.com/)

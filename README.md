# NGINX cache for Traefik

As [Traefik](https://traefik.io/) is a really gread & modern loadbalancer, it [doesn´t feature caching right now](https://github.com/containous/traefik/issues/878). So we need to put something in front of it, that is able to do caching, like [Nginx](https://nginx.org/en/).

## Usage

```text
docker compose up -d
```
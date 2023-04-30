<h1 align="center">
  NGINX cache for Traefik
</h1>

<p align="center">
  <img src="https://user-images.githubusercontent.com/19761269/235366318-045e2e8c-6aab-48d6-a75c-a58895a206e2.png" alt="Nginx and Traefik logos" />
</p>

While [Traefik](https://traefik.io) is a really great and modern load-balancer, it [does not support caching at the moment](https://github.com/traefik/traefik/issues/878).

This project aims to solve _that_ by putting [Nginx](https://nginx.org/en/), a caching load-balancer and reverse proxy in front of it.

This is written for Docker, but the nginx config can be applied to any configuration.

## Features

- Cache is stored and served entirely from memory
- Returns stale items (and refreshes it), so cached content is always returned
- Only static assets are cached... does not affect changing content like API queries
- HTTP errors like `404` are not cached.

Nginx config can be found in the `./nginx` folder.

## Usage

Using Docker Compose:

```text
docker compose up -d
```

Or alternatively, apply nginx.conf to your configuration.

## Persistent caching

To persist the cache across nginx container restarts or system reboots:

1. Remove the `shm_size` line from docker-compose.yml
2. Mount a folder to store caching data, for example:

```bash
mkdir nginx-cache
```

```yml
...
  volumes:
    - ./nginx:/etc/nginx/conf.d:ro
    - ./nginx-cache:/cache
```

3. Change `/dev/shm` in the first line in [`nginx.conf`](./nginx/nginx.conf) to `/cache`, per the example above.
4. Run `docker compose up -d` or restart nginx

## Test caching

To check whether an item is cached or not, check the `X-Cache-Status` header in response headers.

Here's an example with curl:

```bash
~> curl -s -I "http://10.0.0.99/login.html" | grep Cache
X-Cache-Status: MISS

~> curl -s -I "http://10.0.0.99/login.html" | grep Cache
X-Cache-Status: HIT
```

## To-do

- [ ] add HTTPS support

<div style="text-align:center">
  <h1 style="text-align:center;">NGINX cache for Traefik</h1>
</div>


<div style="text-align:center">
  <img src="https://user-images.githubusercontent.com/19761269/235366318-045e2e8c-6aab-48d6-a75c-a58895a206e2.png" style="text-align:center;" />
</div>

While [Traefik](https://traefik.io/) is a really great & modern load-balancer, it [doesn't support ature caching right now](https://github.com/traefik/traefik/issues/878).

As such, putting a caching load-balancer like [Nginx](https://nginx.org/en/) provides a temporary solution to this issue.

## Features

- cache stored and served entirely from memory
- returns stale items (and refreshes it), so cached content is always returned
- only static assets are cached... does not affect dynamic content like API fetches
- HTTP errors like `404` are not cached.

Nginx config can be found in the `./nginx` folder.

To check whether an item is cached or not, check the `X-Cache-Status` header in response headers.

Here's an example with curl:

```bash
~> curl -s -I "http://10.0.0.99/login.html" | grep Cache
X-Cache-Status: MISS

~> curl -s -I "http://10.0.0.99/login.html" | grep Cache
X-Cache-Status: HIT
```

## Usage

```text
docker compose up -d
```
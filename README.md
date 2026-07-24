# skid.misaki.fi

The website for **Skid Jam** — a top-down, drift-happy couch racer for
iPhone, iPad, and Mac. Landing, how-to-play, support, and privacy pages, plus
the host for the app's Universal Links (shared tracks at `/t/<code>`).

The game itself is at [github.com/vlumi/skid](https://github.com/vlumi/skid).

## Build

Static site, built with [Hugo Extended](https://gohugo.io/). No JS except the
shared-track-link fallback on the 404 page; no external assets.

The Hugo version is **pinned** in [`.hugoversion`](.hugoversion) — Hugo is a
build-time tool, not a runtime; bump deliberately and commit only on a clean
local build.

```sh
hugo server        # local preview at http://localhost:1313
hugo               # one-off build into ./public
```

## Deploy

Served by nginx over HTTPS (Let's Encrypt / certbot) on the host for
`skid.misaki.fi`. [`deploy.sh`](deploy.sh) fetches the pinned Hugo Extended
(cached per-version, no root), pulls, and builds straight into the web root.

```sh
./deploy.sh                          # pull, build, publish to /var/www/skid.misaki.fi
WEBROOT=/some/other/path ./deploy.sh # override the publish dir
./deploy.sh --no-pull                # build the working tree as-is
```

See [`nginx.conf.example`](nginx.conf.example) — note the exact-match
location that serves `/.well-known/apple-app-site-association` as JSON with
no redirect (required for Universal Links), and the 404 page that catches
`/t/<code>` shared-track links opened without the app.

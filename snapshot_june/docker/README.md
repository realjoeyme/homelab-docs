# Docker Stacks — Snapshot 2026-06-04

These are the docker-compose stacks running on `docker-01.lan` and `docker-02.lan` as of the snapshot date. Files were copied directly from the hosts; the audit manifest is below.

## Structure

```
docker/
├── docker-01/
│   ├── dozzle/         (log aggregation agent)
│   ├── homepage/       (dashboard UI + 7 config files)
│   ├── nebula-sync/    (Pi-hole config sync)
│   ├── npm/            (Nginx Proxy Manager + Tailscale sidecar)
│   ├── unifi/          (UniFi Network Application + MongoDB)
│   └── watchtower/     (auto-updater)
└── docker-02/
    ├── dozzle/         (log aggregation UI; uses users.yml — excluded)
    ├── pulse/          (homelab monitoring)
    └── watchtower/     (auto-updater)
```

## What's intentionally NOT included

- **All `.env` files** — contain real secrets (Pulse API keys, NPM DB password, UniFi Mongo password, Tailscale auth key, etc.)
- **`dozzle/data/users.yml`** — bcrypt hash of the dozzle admin password
- **UniFi `init-mongo.js`** — contains a literal MongoDB password. The unifi compose file still references it via volume mount; if you reproduce the stack, you must create this file at the path the compose expects, with a password matching `MONGO_PASS` in your `.env`
- **TLS private keys, Let's Encrypt credentials, Pulse API tokens, sessions, encryption keys, Tailscale state, database files, all NPM/UniFi runtime data** — all runtime state, none of it is configuration

## How to reproduce a stack

1. Copy the relevant `docker-compose.yml` (and `config/` if applicable) to a host with Docker + Compose v2
2. Create a `.env` file in the same directory with the required variables (see the `${VAR}` references in the compose)
3. For UniFi: also create `init-mongo.js` with a Mongo user/password matching `MONGO_PASS`
4. For Dozzle (UI): the `users.yml` file is auto-generated on first start — log in with whatever default credentials dozzle creates, then change
5. `docker compose up -d`

## Audit manifest

All 16 files were audited before staging:

| Step | Method | Result |
|---|---|---|
| 1. Source SHA256 | `sha256sum` on each docker host | Recorded (see `audit-manifest.txt`) |
| 2. Hex bypass scan | `xxd -p` + grep for known secret patterns (bcrypt `$2a$`/`$2b$`, `ghp_`, `glpat-`, JWT `eyJ`, AWS `AKIA`, Tailscale `tskey-`, PEM `-----BEGIN`, URL-embedded creds) | No matches in any of the 16 files |
| 3. Post-copy SHA256 | `sha256sum` on staged files | Must match step 1 exactly |
| 4. Final secret sweep | `grep -riE "password\|token\|secret\|key"` on the staged tree | Only matches are `password: {{HOMEPAGE_VAR_*_TOKEN}}` placeholders in `services.yaml` and the `volume:` keyword in compose files |

Full audit log: see commit message of the snapshot commit.

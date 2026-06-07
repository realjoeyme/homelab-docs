# Docker Stacks - Snapshot 2026-06-04

These are the docker-compose stacks running on docker-01.lan and docker-02.lan as of the snapshot date. Files were copied directly from the hosts.

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
    ├── dozzle/         (log aggregation UI)
    ├── pulse/          (homelab monitoring)
    └── watchtower/     (auto-updater)
```

## What's intentionally NOT included

* All `.env` files - Contain real secrets (Pulse API keys, NPM DB password, UniFi Mongo password, Tailscale auth key, etc.)
* `dozzle/data/users.yml` - bcrypt hash of the dozzle admin password
* UniFi `init-mongo.js` - Contains a literal MongoDB password. The unifi compose file still references it via volume mount; if you reproduce the stack, you must create this file at the path the compose expects, with a password matching MONGO_PASS in your .env
* TLS private keys, Let's Encrypt credentials, runtime databases, and state data.

## How to reproduce a stack

1. Copy the relevant docker-compose.yml (and config/ if applicable) to a host with Docker + Compose v2.
2. Create a .env file in the same directory with the required variables (see the ${VAR} references in the compose).
3. For UniFi: Create init-mongo.js with a Mongo user/password matching MONGO_PASS.
4. Run docker compose up -d.

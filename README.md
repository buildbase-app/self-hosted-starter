# Buildbase Self-Hosted Starter

A ready-to-run [Docker Compose](https://docs.docker.com/compose/) stack for self-hosting **[Buildbase](https://buildbase.app)** on your own infrastructure. Clone it, fill in three secrets, and run one command.

> **What this is:** the four-service Buildbase stack (Tenant Server + Client + Auth + MongoDB + Redis) wired together. The Central Server stays Buildbase-managed — your stack talks to it for licensing only, never sends it your app data.

**Learn more about self-hosting Buildbase:**
- 🌐 Overview — **https://www.buildbase.app/self-hosted**
- 📖 Full docs — **https://docs.buildbase.app/self-hosted/overview**

### Deploy with AI assistance

There's an official Claude skill that walks you through deploying and operating this stack — the architecture, env vars, production hardening, and doc-grounded diagnostics. Install it in Claude Code:

```
/plugin marketplace add buildbase-app/claude-skill
/plugin install buildbase-selfhost@buildbase-skills
```

Then just ask: *"Deploy Buildbase self-hosted with Docker Compose."* Skill repo: **https://github.com/buildbase-app/claude-skill**

---

## What's in here

| File | Purpose |
|------|---------|
| `docker-compose.selfhost.yml` | The full stack — Mongo, Redis, tenant-server, client, auth |
| `.env.selfhost.example` | Environment template — **copy this**, never commit your filled-in copy |
| `Dockerfile.tenant-server` | Optional tenant-server image with the `busboy` upload dependency |
| `nginx-lb.conf` | Internal load-balancer config for production (replicas + sticky sessions) |

---

## Prerequisites

- Linux host (Ubuntu 20.04+ recommended), 2 vCPU / 2 GB RAM minimum
- Docker Engine 20+ and Compose v2
- A self-hosted org + **Installation** created at **https://console.buildbase.app**
  (gives you `INSTALLATION_API_KEY` + `INSTALLATION_ID`)

---

## Quick start (local)

```bash
# 1. Get the files
git clone https://github.com/buildbase-app/self-hosted-starter
cd self-hosted-starter

# 2. Create your env file from the template
cp .env.selfhost.example .env.selfhost

# 3. Fill in INSTALLATION_API_KEY + INSTALLATION_ID (from the dashboard),
#    and generate the four secrets:
openssl rand -hex 32   # run 4x — one each for JWT_PASS, DB_ENCRYPTION_KEY,
                       # SECRET_KEY, OAUTH2_SECRET
#    ...then edit .env.selfhost and paste them in.

# 4. Bring the stack up
docker compose -f docker-compose.selfhost.yml --env-file .env.selfhost up -d

# 5. Verify
docker compose -f docker-compose.selfhost.yml ps
curl http://localhost:4101/api/ready     # -> {"ready": true}
```

Then open the client at **http://localhost:4100** and complete the setup wizard in the dashboard (server URL → **Test Connection** → **Complete Setup**).

| Service | Default URL |
|---------|-------------|
| Client (dashboard) | http://localhost:4100 |
| Tenant Server (API) | http://localhost:4101 |
| Auth Portal | http://localhost:4103 |

---

## Going to production

The quick-start stack runs as-is, but for a real deployment:

1. **Set real secrets.** Every one of `JWT_PASS`, `DB_ENCRYPTION_KEY`, `SECRET_KEY`, `OAUTH2_SECRET` must be a unique `openssl rand -hex 32` value — the compose file's `localdev_..._do_not_use_in_production` fallbacks are for booting locally only.
2. **Use real public URLs** with SSL (set `CLIENT_URL` / `TENANT_SERVER_URL` / `AUTH_URL` to your `https://` domains). The "Test Connection" step needs the URL to actually reach your tenant server.
3. **Front it with TLS** — terminate SSL at a host Nginx (e.g. via certbot/Caddy), and use `nginx-lb.conf` to load-balance tenant-server replicas with sticky sessions.
4. **Back up MongoDB and Redis** — this is your responsibility. Buildbase's docs don't ship a backup procedure; use standard MongoDB/Redis practice.

See the production guide: **https://docs.buildbase.app/self-hosted/production**

---

## Resources

| | Link |
|---|---|
| Self-hosted overview (main site) | https://www.buildbase.app/self-hosted |
| Self-hosting docs | https://docs.buildbase.app/self-hosted/overview |
| Dashboard (create your Installation) | https://console.buildbase.app |
| Claude self-hosting skill | https://github.com/buildbase-app/claude-skill |
| &nbsp; ↳ install | `/plugin install buildbase-selfhost@buildbase-skills` |

---

## ⚠️ Security

- **Never commit `.env.selfhost`.** It's already in `.gitignore`. Only the `.example` template belongs in git.
- If a secret (especially `INSTALLATION_API_KEY`) ever lands in a commit or a public place, **rotate it** — treat it as compromised.
- The default compose does **not** expose MongoDB or Redis to the host; they're only reachable on the internal Docker network. Keep it that way.

---

## Contributing

Contributions are welcome via fork → PR. See **[CONTRIBUTING.md](./CONTRIBUTING.md)** for the workflow, and **[SECURITY.md](./SECURITY.md)** for reporting vulnerabilities (don't open public issues for those).

## License

MIT — see [LICENSE](./LICENSE). Buildbase and its images belong to their owner; this is an independent starter.

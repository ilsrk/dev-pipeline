# Docker Compose — reference

Short Compose V2 cheat sheet. Details: https://docs.docker.com/compose/compose-file/

## File names and merge

| File | Role |
|------|------|
| `compose.yaml` / `compose.yml` | Preferred name (Compose finds it automatically) |
| `docker-compose.yml` | Equivalent if the repo already uses it |
| `compose.override.yaml` | Auto-merged with the base on `docker compose up` |
| `compose.*.yaml` | Explicit via `-f` / `COMPOSE_FILE` (ci, prod, tools) |

Multiple files:

```bash
docker compose -f compose.yaml -f compose.ci.yaml config
# or
COMPOSE_FILE=compose.yaml:compose.ci.yaml docker compose config
```

Order of `-f` / `COMPOSE_FILE`: left to right; later overrides earlier.

## Useful service fields

| Field | Purpose |
|-------|---------|
| `image` / `build` | Prebuilt image and/or build; often both (`build` + tagged `image`) |
| `env_file` / `environment` | Files and explicit vars (`environment` wins) |
| `ports` | `"HOST:CONTAINER"`; host port via `${VAR:-default}` |
| `volumes` | Named volume or bind `./path:/path` |
| `networks` | Attach to user-defined networks |
| `depends_on` | Start order; with `condition: service_healthy` / `service_started` |
| `healthcheck` | Readiness for depends_on and orchestration |
| `profiles` | Do not start the service until the profile is selected |
| `restart` | `no` / `always` / `unless-stopped` / `on-failure` |
| `user` / `read_only` / `cap_drop` | Hardening |
| `command` / `entrypoint` | Override startup |

Never set `network_mode: host` (host network of the working machine).

## depends_on + healthcheck

```yaml
services:
  db:
    image: postgres:16
    healthcheck:
      test: ["CMD-SHELL", "pg_isready -U $$POSTGRES_USER"]
      interval: 5s
      timeout: 3s
      retries: 10
    volumes:
      - db_data:/var/lib/postgresql/data

  api:
    build: ./api
    depends_on:
      db:
        condition: service_healthy
    environment:
      DATABASE_URL: postgres://app:app@db:5432/app
      PUBLIC_URL: https://${VIRTUAL_HOST}

volumes:
  db_data:
```

Inside the network, talk by **service name** (`db`, `api`). `localhost` inside a container is that container itself.

## Profiles

```yaml
services:
  app:
    image: myapp:dev
    # no profile — always in the default project

  mailhog:
    image: mailhog/mailhog
    profiles: [dev]
```

```bash
docker compose --profile dev up -d
# or COMPOSE_PROFILES=dev
```

## Override for the local machine

```yaml
# compose.override.yaml (often gitignored or only for dev)
services:
  app:
    volumes:
      - ./src:/app/src
    environment:
      DEBUG: "1"
```

## Variables and secrets

- Substitution in YAML: `${VAR}` / `${VAR:-default}`
- `.env` next to compose is picked up for **compose-file substitution** and as default container env (behavior depends on version/settings — duplicate important values via `env_file` / `environment`)
- Do not commit secrets; `.env.example` without real values
- For sensitive data — Docker secrets / external secret manager, not plaintext in git

## Common commands (for debugging; in project docs — via task)

```bash
docker compose config              # validate + rendered YAML
docker compose up -d               # detach
docker compose up -d --build       # rebuild
docker compose ps
docker compose logs -f --tail=200 SERVICE
docker compose exec SERVICE sh
docker compose down                # containers + default network
docker compose down -v             # also volumes (destructive — only on explicit request)
```

## Network and VIRTUAL_HOST

| Context | What to use |
|---------|-------------|
| URL for browser / client / OAuth callback | `https://${VIRTUAL_HOST}` |
| Service → service (app traffic) | `https://${*_VIRTUAL_HOST}` (via Traefik / virtual host) |
| Healthcheck inside the container | `https://${SERVICE_VIRTUAL_HOST}:${SERVICE_CONTAINER_PORT}/…` (`curl -fsSk` for local/dev certs) |

Never use `http://`, `127.0.0.1`, or `localhost` in healthcheck URLs.

## Taskfile wiring

Minimum: `up`, `down`, `logs`, `status`. Variables `COMPOSE_FILE`, `SERVICE`, `dir:` when needed. See skill `taskfile`.

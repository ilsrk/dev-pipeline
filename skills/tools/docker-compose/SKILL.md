---
name: docker-compose
description: >-
  Creates, extends, debugs, and refactors Docker Compose stacks (compose.yaml /
  docker-compose.yml). Use when working with Docker Compose, services, networks,
  volumes, Compose profiles/overrides, local multi-container stacks, or when the
  user mentions docker compose, compose.yaml, docker-compose, VIRTUAL_HOST, or
  wrapping Compose in Taskfile — not Dockerfile/build alone (see docker-image),
  not one-off docker run alone (see docker), not Redis data/CLI alone (see redis).
---

# Docker Compose

Local and dev stacks via `docker compose` (Compose V2 plugin). After any new or changed service, in the same change initialize **both** environment variables **and** a Taskfile. Without an `.env.example` (contract) + `task` wrappers, the service is incomplete. Details: skills `env` and `taskfile`. Rule: `docker-service-env-taskfile`.

## Routing

| Task | Skill |
|------|--------|
| `Dockerfile`, `docker build`, tags, image hardening | `docker-image` |
| One-off `docker run`, container debugging, volume/network, prune | `docker` |
| `Taskfile`, `task up` / `build` | `taskfile` |
| `compose.yaml`, services, profiles, VIRTUAL_HOST in a stack | **this skill** |

If a change touches both the image and compose — apply both skills in the same change.

## When to apply

1. Create or extend `compose.yaml` / `docker-compose.yml` / overrides
2. Debug a failing stack (logs, healthcheck, network, dependencies)
3. Move tools/runtimes into compose instead of installing on the host
4. Align hosts/URLs with `VIRTUAL_HOST` and HTTPS for client endpoints

## Workflow

1. Find existing compose files and repo style (`compose.yaml`, `docker-compose.yml`, `compose.*.yaml`, `.env.example`)
2. Extend the existing stack; a new file only if the repo already does that (service subdirectory, separate profile)
3. File names: prefer `compose.yaml` (or the repo’s established `docker-compose.yml`)
4. Initialize env immediately: `.env.example` (+ `.gitignore` for `.env`), `env_file` / `environment` in compose, `VIRTUAL_HOST` / `PUBLIC_URL` where needed
5. Add/update Taskfile immediately with `up` / `down` / `logs` / `status`, plus `env` / `env:force` (and `build` / `shell` as needed); `up` with `deps: [env]`
6. Validate: `docker compose -f <file> config` (must succeed)
7. Document startup as `task …`, not raw `docker compose …`, when a task already exists

## Rules

- CLI: `docker compose` (not the legacy `docker-compose` binary), unless the user/repo explicitly requires otherwise
- Secrets not in git: `.env` in `.gitignore`; in the repo — `.env.example` with placeholders
- Hosts: for base URL / callback / docs / external curl / healthchecks — `$VIRTUAL_HOST` / `${*_VIRTUAL_HOST}`, never `localhost`, `127.0.0.1`, or other raw IPs
- Client URLs and healthchecks: scheme `https://` only — never `http://` (use `curl -fsSk` when the service presents a local/dev certificate)
- Healthcheck URL form: `https://${SERVICE_VIRTUAL_HOST}:${SERVICE_CONTAINER_PORT}/…` (same pattern as `core-backend` / `landing-frontend` in this repo)
- Images: pin tags/digests where appropriate; do not run `docker pull` yourself — name the required image and ask the user to pull if it is missing locally
- Do not install packages/CLIs on the host: compilers, linters, runtimes — via a compose service or `docker run`
- `depends_on` + `condition: service_healthy` instead of “wait N seconds”
- Ports: in `ports:` never hardcode numbers — only `${VAR}` / `${VAR:-default}`; same names in `.env.example` (rule `docker-compose-ports-from-env`)
- Named volumes for data; bind mounts — for code/configs in dev
- Networks: explicit user-defined network when services must resolve by service name
- NEVER use the host network of the working machine (`network_mode: host` / `--network host`)
- Overrides: `compose.override.yaml` for local differences; do not commit machine-specific overrides without need
- Profiles: optional services (`profiles: [tools]`) instead of always `up`ing the whole zoo
- Env values live in env files and are imported into compose; do not invent inline secrets in compose YAML

## Minimal service + Taskfile

```yaml
# compose.yaml
services:
  app:
    build: .
    image: ${COMPOSE_PROJECT_NAME:-app}:dev
    env_file: .env
    environment:
      VIRTUAL_HOST: ${VIRTUAL_HOST}
      PUBLIC_URL: https://${VIRTUAL_HOST}
    ports:
      - "${APP_PORT:-3000}:${APP_CONTAINER_PORT:-3000}"
    # healthcheck — https + *_VIRTUAL_HOST + container port (never http://, 127.0.0.1, localhost)
    healthcheck:
      test:
        [
          "CMD",
          "curl",
          "-fsSk",
          "https://${APP_VIRTUAL_HOST}:${APP_CONTAINER_PORT:-3000}/health",
        ]
      interval: 10s
      timeout: 3s
      retries: 5
      start_period: 20s
    networks: [app]

networks:
  app:
```

In parallel — tasks `up` / `down` / `logs` / `status` with `COMPOSE_FILE` and `SERVICE` (templates in skill `taskfile`).

## Stack debugging

1. `docker compose -f <file> ps` / `task status`
2. `task logs` (or `docker compose logs -f --tail=200 <svc>`)
3. `docker compose config` — syntax and env substitution
4. Health: `docker inspect --format='{{json .State.Health}}' <container>`
5. Network: services reach each other by **service name**, not `localhost` inside the container
6. Permissions/volume: check UID and mount path

## Anti-patterns

- Service without `.env.example` / without wiring env into compose
- Service without Taskfile wrappers (`up` / `down` / `logs` / `status` / `env`)
- Hardcoded ports in `ports:` (`"3000:3000"`, `"8080:80"`) instead of `${APP_PORT}` / `${APP_CONTAINER_PORT}`
- `localhost` / `127.0.0.1` / raw IPs in PUBLIC_URL, callbacks, curl examples, or healthchecks
- `http://` for client endpoints or healthchecks
- Secrets in compose YAML or a committed `.env`
- `latest` without reason in prod/CI
- `depends_on` without healthcheck when DB/API readiness is required
- `network_mode: host` / host network of the working machine
- Installing a toolchain on the host “because it’s faster” when a compose service exists or is needed
- Docs with raw `docker compose …` when `task …` already exists

## Additional resources

- Syntax, overrides, profiles: [reference.md](reference.md)
- One-off `docker run` / container debugging: skill `docker`
- Images and Dockerfiles: skill `docker-image`
- Traefik labels / ACME edge: skill `traefik`
- Nginx vhost / proxy_pass: skill `nginx`
- Official docs: https://docs.docker.com/compose/

---
name: docker
description: >-
  Works with Docker Engine CLI and container runtime: docker run/exec/ps/logs,
  volumes, networks, prune, and container debugging. Use when the user mentions
  docker, container, docker run, docker exec, docker logs, docker volume, docker
  network, or one-off tools via containers — not Dockerfile/build alone (see
  docker-image), not Compose stacks alone (see docker-compose), not Taskfile
  wrappers alone (see taskfile).
---

# Docker (Engine / CLI)

Containers and Docker Engine CLI (`docker run`, `exec`, `ps`, `logs`, volumes, networks). Images/Dockerfile — skill `docker-image`; service stacks — `docker-compose`; `task …` wrappers — `taskfile`.

## Routing

| Task | Skill |
|------|--------|
| `Dockerfile`, `docker build`, tags, image hardening | `docker-image` |
| `compose.yaml`, services, profiles, VIRTUAL_HOST in a stack | `docker-compose` |
| `Taskfile`, `task up` / `build` | `taskfile` |
| One-off `docker run`, container debugging, volume/network, prune | **this skill** |

If a change touches both the image and compose — apply both skills in the same change.

## When to apply

1. Run a utility/runtime in a container instead of installing on the host
2. Debug a running or failed container (logs, exec, inspect)
3. Manage volumes / networks / cleanup
4. Choose safe `docker run` flags (user, mounts, network, restart)

## Workflow

1. Clarify the goal: one-off command, debugging, cleanup, or a long-lived service
2. Long-lived service / multiple containers → skill `docker-compose` (+ Taskfile), not bare `docker run -d` in docs
3. Image build → skill `docker-image`
4. Before destructive ops (`rm -f`, `volume rm`, `system prune`) — briefly warn and ask, unless the user explicitly requested cleanup
5. Missing image: **name the tag/digest and ask** for `docker pull …`; do not run `docker pull` yourself without an explicit request in this turn
6. Prefer `task …` in replies when a task already exists; otherwise show the exact `docker …` command

## Rules

- Do not install packages/CLIs on the host: compilers, linters, runtimes — via `docker run` / compose (no-local-package-install rule)
- Hosts for public URLs / callbacks / client curl / healthchecks: `$VIRTUAL_HOST` / `${*_VIRTUAL_HOST}`, scheme `https://` — never `localhost`, `127.0.0.1`, or `http://`
- Healthchecks: `https://${SERVICE_VIRTUAL_HOST}:${SERVICE_CONTAINER_PORT}/…` with `curl -fsSk` when using local/dev certificates
- Pin image tags/digests; `latest` — only deliberately
- Mounts: use `:ro` for read-only configs/secrets; do not mount all of `$HOME` or the Docker socket without need and a warning
- User: prefer `--user` / non-root in the image; root — only when unavoidable
- Network: user-defined bridge for container-to-container name DNS
- NEVER use the host network of the working machine
- Secrets: `-e` / `--env-file` (file not in git) / runtime secret; do not bake into the image or log them
- Cleanup: `docker system prune` and `volume prune` destroy data — only on explicit request

## One-off instead of the host

```bash
# Example: utility in a container, workdir = current project
docker run --rm -it \
  -v "$PWD":/work -w /work \
  -e VIRTUAL_HOST \
  <image>:<tag> <command>
```

Node/Python/Go CLI — same skeleton: `--rm`, code mount, pinned image. For a long-lived service → compose + Taskfile.

## Common commands

| Action | Command |
|--------|---------|
| List | `docker ps -a` |
| Logs | `docker logs -f --tail=200 <id\|name>` |
| Shell | `docker exec -it <id\|name> sh` (or `bash` if present) |
| Inspect | `docker inspect <id\|name>` |
| Stop/start | `docker stop` / `docker start` |
| Remove container | `docker rm` (add `-f` only deliberately) |
| Stats | `docker stats` |
| Volumes | `docker volume ls` / `inspect` / `rm` |
| Networks | `docker network ls` / `inspect` / `rm` |
| Disk/garbage | `docker system df` ; prune — only on request |

## Container debugging

1. `docker ps -a` — status and exit code
2. `docker logs --tail=200 <c>` — failure reason
3. `docker inspect --format='{{.State.Status}} {{.State.ExitCode}} {{.State.Error}}' <c>`
4. If the container is alive: `docker exec -it <c> sh` — process, DNS, mount, env
5. Network: from container A to B — by **name** (compose) or IP; not the neighbor's `localhost`
6. Volume/permissions: UID in the container vs file owner on the host
7. Broken image/build → skill `docker-image`; compose stack → `docker-compose`

## Anti-patterns

- Agent running `docker pull` without an explicit user request
- Installing a toolchain on the host instead of `docker run` / compose
- Documenting daemons via bare `docker run -d` when compose + `task up` is needed
- `localhost` / `127.0.0.1` / `http://` in PUBLIC_URL, callbacks, client curl, or healthchecks
- Docker socket mount (`/var/run/docker.sock`) without clear need and risk acknowledgment
- `--network host` / `network_mode: host` (host network of the working machine)
- `docker system prune -a --volumes` "just in case"
- Root + writable bind mount of the whole repo as the only way to "fix permissions"

## Additional resources

- Common `run`/`exec` flags, prune, troubleshooting: [reference.md](reference.md)
- Images and Dockerfiles: skill `docker-image`
- Compose stacks: skill `docker-compose`
- Official docs: https://docs.docker.com/engine/reference/commandline/cli/

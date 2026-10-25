# Docker CLI — reference

Read on demand; keep only essentials in SKILL.md.

## Useful `docker run` flags

| Flag | Purpose |
|------|---------|
| `--rm` | Remove the container after exit (one-off) |
| `-i` `-t` / `-it` | Interactive TTY |
| `-d` | Detached (prefer compose for daemons) |
| `-v host:container[:ro]` | Bind mount |
| `--mount type=volume,src=…,dst=…` | Named volume |
| `-w /path` | Workdir |
| `-e VAR=val` / `--env-file` | Env (do not commit the file) |
| `-p host:container` | Publish port |
| `--network name` | User-defined network |
| `--user uid:gid` | Non-root |
| `--read-only` | Read-only root FS (+ tmpfs for `/tmp`) |
| `--cap-drop ALL` + needed `--cap-add` | Minimal capabilities |
| `--security-opt no-new-privileges` | No privilege escalation |
| `--restart unless-stopped` | Restart policy (or in compose) |
| `--name` | Stable name |
| `--pull never` | Do not pull the image implicitly (strict policy) |

## Inspect — quick templates

```bash
docker inspect --format='{{json .State}}' <c>
docker inspect --format='{{range .Mounts}}{{.Type}} {{.Source}} -> {{.Destination}}{{"\n"}}{{end}}' <c>
docker inspect --format='{{range .NetworkSettings.Networks}}{{.IPAddress}}{{end}}' <c>
docker inspect --format='{{json .Config.Env}}' <c>
```

Health (if configured):

```bash
docker inspect --format='{{json .State.Health}}' <c>
```

## Volumes

```bash
docker volume ls
docker volume inspect <name>
docker volume create <name>
docker volume rm <name>          # only if unused
docker volume prune              # UNUSED volumes — data will be lost
```

Bind mount ≠ volume: host path vs Docker object. For DB data — prefer a named volume.

## Networks

```bash
docker network ls
docker network create <name>
docker network connect <net> <c>
docker network inspect <net>
```

DNS inside a user-defined network: container name / compose service name → IP. On the default `bridge`, name linking is limited — for multi-container use a compose network.

NEVER use the host network of the working machine (`--network host` / `network_mode: host`).

## Cleanup (destructive)

Only on an explicit user request:

```bash
docker system df
docker container prune           # stopped containers
docker image prune               # dangling images
docker system prune              # stopped + dangling + unused networks
docker system prune -a           # + unused images
docker volume prune              # unused volumes
```

Before prune, show `docker system df` and what will be affected.

## Copy / files

```bash
docker cp <c>:/path/in ./local
docker cp ./local <c>:/path/in
```

For regular development prefer a bind mount, not `docker cp`.

## Resources

```bash
docker stats --no-stream
docker update --memory=512m --cpus=1 <c>   # rare; prefer limits in compose
```

## Related skills

- Image build and layers → `docker-image` / its `reference.md`
- Services, healthcheck, profiles → `docker-compose` / its `reference.md`
- `task up` / `logs` / `shell` → `taskfile`

---
name: Bug report
about: Something in the starter doesn't work as documented
title: "[bug] "
labels: bug
---

## What happened

<!-- Clear description of the bug. -->

## Steps to reproduce

1.
2.
3.

## Expected behavior

<!-- What you expected instead. -->

## Environment

- Host OS / arch:
- Docker version (`docker --version`):
- Compose version (`docker compose version`):
- Buildbase image tags (from `docker-compose.selfhost.yml`):

## `/api/ready` output

```
# curl http://localhost:4101/api/ready
```

## Logs (redact secrets!)

```
# docker compose -f docker-compose.selfhost.yml logs --tail=50 tenant-server
```

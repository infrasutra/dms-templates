# Redis Template

Redis is an open-source, in-memory data structure store used as a database, cache, message broker, and streaming engine.

## Details

| Property | Value |
|----------|-------|
| Image | `redis:7-alpine` |
| Port | 6379 |
| Category | Cache |
| Storage | 5Gi |

## Configuration Variables

| Variable | Required | Default | Description |
|----------|----------|---------|-------------|
| `REDIS_PASSWORD` | No | (auto-generated) | Redis authentication password |

## Storage

This template provisions a 5Gi persistent volume mounted at `/data` for Redis data persistence. AOF (Append Only File) persistence is enabled by default for durability.

## Persistence

The template configures Redis with `--appendonly yes` for AOF persistence, ensuring data survives restarts.

## Health Check

The template uses `redis-cli ping` for health checking, ensuring Redis is ready to accept connections.

## Resource Defaults

| Resource | Request | Limit |
|----------|---------|-------|
| CPU | 50m | 500m |
| Memory | 128Mi | 512Mi |

## Connection

Once deployed, connect to Redis using:

```bash
redis-cli -h <service-host> -p 6379 -a $REDIS_PASSWORD
```

Or from your application:

```
redis://:$REDIS_PASSWORD@<service-host>:6379
```

## Notes

- Password authentication is enabled by default
- AOF persistence ensures data durability
- Alpine-based image for smaller footprint
- Suitable for caching, session storage, and message queuing

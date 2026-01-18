# PostgreSQL Template

PostgreSQL is a powerful, open-source object-relational database system with a strong reputation for reliability, feature robustness, and performance.

## Details

| Property | Value |
|----------|-------|
| Image | `postgres:16-alpine` |
| Port | 5432 |
| Category | Database |
| Storage | 10Gi |

## Configuration Variables

| Variable | Required | Default | Description |
|----------|----------|---------|-------------|
| `POSTGRES_USER` | No | `postgres` | PostgreSQL superuser username |
| `POSTGRES_PASSWORD` | Yes | (auto-generated) | PostgreSQL superuser password |
| `POSTGRES_DB` | No | `postgres` | Default database name to create |

## Storage

This template provisions a 10Gi persistent volume mounted at `/var/lib/postgresql/data` for database storage.

## Health Check

The template uses `pg_isready` for health checking, ensuring the database is ready to accept connections before marking the service as healthy.

## Resource Defaults

| Resource | Request | Limit |
|----------|---------|-------|
| CPU | 100m | 1000m |
| Memory | 256Mi | 1Gi |

## Connection

Once deployed, connect to PostgreSQL using:

```bash
psql -h <service-host> -p 5432 -U $POSTGRES_USER -d $POSTGRES_DB
```

## Notes

- The password is auto-generated if not provided and stored as a secret
- Data persists across restarts using the provisioned volume
- Alpine-based image for smaller footprint

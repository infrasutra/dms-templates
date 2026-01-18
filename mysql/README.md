# MySQL Template

MySQL is the world's most popular open-source relational database management system, known for its speed, reliability, and ease of use.

## Details

| Property | Value |
|----------|-------|
| Image | `mysql:8.0` |
| Port | 3306 |
| Category | Database |
| Storage | 10Gi |

## Configuration Variables

| Variable | Required | Default | Description |
|----------|----------|---------|-------------|
| `MYSQL_ROOT_PASSWORD` | Yes | (auto-generated) | MySQL root user password |
| `MYSQL_USER` | No | `app` | MySQL application user username |
| `MYSQL_PASSWORD` | No | (auto-generated) | MySQL application user password |
| `MYSQL_DATABASE` | No | `app` | Default database name to create |

## Storage

This template provisions a 10Gi persistent volume mounted at `/var/lib/mysql` for database storage.

## Health Check

The template uses `mysqladmin ping` for health checking, ensuring the database is ready to accept connections before marking the service as healthy.

## Resource Defaults

| Resource | Request | Limit |
|----------|---------|-------|
| CPU | 100m | 1000m |
| Memory | 512Mi | 2Gi |

## Connection

Once deployed, connect to MySQL using:

```bash
mysql -h <service-host> -P 3306 -u $MYSQL_USER -p$MYSQL_PASSWORD $MYSQL_DATABASE
```

Or as root:

```bash
mysql -h <service-host> -P 3306 -u root -p$MYSQL_ROOT_PASSWORD
```

## Notes

- Both root and application user credentials are auto-generated if not provided
- Passwords are stored as secrets
- Data persists across restarts using the provisioned volume
- The application user has full privileges on the specified database

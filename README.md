# DMS Templates

Official default templates for the DMS (Deployment Management Stack) platform. These templates provide pre-configured infrastructure services that can be deployed through DMS.

## Available Templates

| Template | Description | Versions | Default |
|----------|-------------|----------|---------|
| [PostgreSQL](./postgres/) | PostgreSQL database with Alpine Linux | 13, 14, 15, 16, 17, 18 | 16 |
| [Redis](./redis/) | Redis in-memory data store with AOF persistence | 6, 7, 8 | 7 |
| [MySQL](./mysql/) | MySQL relational database | 5.6, 5.7, 8.0, 8.4 | 8.0 |

## Usage

Templates can be registered in DMS and deployed to any stage within your projects. Each template follows the DMS YAML specification v1.

### Deploying a Template

```bash
# Deploy with default version
dmsctl deploy --template postgres

# Deploy with specific version
dmsctl deploy --template postgres --set VERSION=17

# Deploy with custom configuration
dmsctl deploy --template postgres --set VERSION=16 --set POSTGRES_DB=myapp
```

### Version Selection

All templates support the `VERSION` variable to select which software version to deploy. The version can be overridden:

1. **At deploy time** via `--set VERSION=<version>`
2. **In dms.yaml** via variables section
3. **In DMS UI** via the version dropdown

If not specified, the `defaultVersion` is used.

### Registering a Template

Templates from this repository can be registered in DMS by pointing to the GitHub repository URL.

### Template Structure

Each template directory contains:
- `dms-template.yaml` - The DMS template specification file
- `README.md` - Documentation for the template including configuration options

## Template Specification

All templates follow the DMS YAML v1 specification:

```yaml
version: "1"
kind: template

# Template metadata
name: template-name
description: Template description
icon: https://example.com/icon.png
category: database
tags:
  - tag1
  - tag2

# Version support
versions:
  - "8"
  - "7"
  - "6"
defaultVersion: "7"

# Variables (VERSION is reserved for version selection)
variables:
  - key: VERSION
    type: string
    description: Software version to deploy
    configurable: true
    options:
      - "8"
      - "7"
      - "6"
    default: "7"
  - key: MY_VAR
    type: secret
    generator: password

# Service definitions
services:
  - name: service-name
    type: worker
    image: "image:${VERSION}"
    ports:
      - port: 1234
    env:
      - key: MY_VAR
    volumes:
      - name: data
        path: /data
        size: 10240
    health_check:
      port: 1234
      interval: 10
      timeout: 5
      retries: 3
```

## Key Concepts

### VERSION Variable

The `VERSION` variable is a reserved variable that controls which software version to deploy:

- Defined in `versions` array at the spec level
- Default specified by `defaultVersion`
- Can be overridden at deploy time
- Used in image tags via `${VERSION}` interpolation

### Variable Types

| Type | Description |
|------|-------------|
| `string` | Plain text value |
| `secret` | Sensitive value stored in Kubernetes secrets |
| `integer` | Numeric value |
| `boolean` | True/false value |
| `reference` | Reference to another service's output |

### Generators

| Generator | Description |
|-----------|-------------|
| `password` | Generate a random password |
| `secret` | Generate a random secret string |
| `uuid` | Generate a UUID |
| `random_int` | Generate a random integer |

### TCP Exposure (External Access)

Database and cache templates support optional external access via Gateway API TLSRoute. By default, services are internal-only for security.

```yaml
tcp_exposure:
  enabled: ${EXPOSE_EXTERNAL}  # Controlled by EXPOSE_EXTERNAL variable
  hostname: "${NAME}-${STAGE}.${DOMAIN}"
  port: 5432
  tls:
    mode: terminate  # Gateway terminates TLS, forwards plain TCP
  gateway:
    name: dms-gateway
    namespace: dms-system
```

**TLS Modes:**

| Mode | Description | Use Case |
|------|-------------|----------|
| `terminate` | Gateway terminates TLS, forwards plain TCP | Standard databases without built-in TLS |
| `passthrough` | Gateway passes encrypted traffic to backend | Services with built-in TLS support |

**Enabling External Access:**

```bash
# Enable external access at deploy time
dmsctl deploy --template postgres --set EXPOSE_EXTERNAL=true
```

## Contributing

To add a new template:

1. Create a new directory with the template name
2. Add a `dms-template.yaml` following the specification
3. Add a `README.md` documenting the template
4. Submit a pull request

## License

MIT License - see LICENSE file for details.

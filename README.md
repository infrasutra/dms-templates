# DMS Templates

Official default templates for the DMS (Deployment Management Stack) platform. These templates provide pre-configured infrastructure services that can be deployed through DMS.

## Available Templates

| Template | Description | Version |
|----------|-------------|---------|
| [PostgreSQL](./postgres/) | PostgreSQL 16 database with Alpine Linux | 16-alpine |
| [Redis](./redis/) | Redis 7 in-memory data store with AOF persistence | 7-alpine |
| [MySQL](./mysql/) | MySQL 8.0 relational database | 8.0 |

## Usage

Templates can be registered in DMS and deployed to any stage within your projects. Each template follows the DMS YAML specification v1.

### Registering a Template

Templates from this repository can be registered in DMS by pointing to the raw GitHub URL of the `dms-template.yaml` file.

### Template Structure

Each template directory contains:
- `dms-template.yaml` - The DMS template specification file
- `README.md` - Documentation for the template including configuration options

## Template Specification

All templates follow the DMS YAML v1 specification:

```yaml
version: "1"
kind: template
metadata:
  name: template-name
  description: Template description
  category: database|cache|messaging|...
  tags: [tag1, tag2]
spec:
  image: image:tag
  port: container-port
  variables:
    - name: VAR_NAME
      required: true|false
      default: default-value
      generate: password|uuid|...
  volumes:
    - name: volume-name
      mountPath: /path/in/container
      size: 10Gi
  healthcheck:
    path: /health
    port: 8080
    initialDelaySeconds: 30
    periodSeconds: 10
```

## Contributing

To add a new template:

1. Create a new directory with the template name
2. Add a `dms-template.yaml` following the specification
3. Add a `README.md` documenting the template
4. Submit a pull request

## License

MIT License - see LICENSE file for details.

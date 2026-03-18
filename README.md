# Docker Compose Applications

This repository contains multiple Docker Compose configurations for deploying .NET 8.0 applications with Traefik reverse proxy.

## Structure

```
DockerCompose/
├── local-postgres/          # Local PostgreSQL instance
├── local-sqlserver/         # Local SQL Server 2025 instance
└── remote/                  # Production applications
    ├── basic-todo-list/
    ├── data-viewer/
    ├── image-gallery/
    ├── jwt-authentication/
    ├── monthly-report/
    ├── postgres/
    ├── product-catalog/
    ├── real-time-chat/
    ├── social-media-feed/
    ├── task-scheduler/
    ├── traefik/            # Reverse proxy & load balancer
    └── weather-forecast/
```

## Local Databases

Standalone database instances for local development.

| Folder | Database | Port | User | Password |
|---|---|---|---|---|
| `local-postgres/` | PostgreSQL | `5432` | `postgres` | `postgres` |
| `local-sqlserver/` | SQL Server 2025 (Developer) | `1433` | `sa` | `YourStrong!Passw0rd` |

Start a local database:

```bash
cd local-postgres     # or local-sqlserver
docker compose up -d
```

## Applications

Each application in `remote/` is a .NET 8.0 ASP.NET application deployed with:

- **Traefik** reverse proxy for routing and SSL/TLS
- **Environment variables** from `.env` files
- **Automatic HTTPS** with Let's Encrypt certificates
- **Domain routing** on `*.paweldywan.com`

## Deployment

Each application requires:

1. `.env` file with configuration variables:
   ```bash
   NAME=AppName
   STACK_NAME=app-name
   ```

2. `app/` folder containing the compiled .NET application

3. Docker networks:
   - `webnet` - External network for Traefik routing
   - `db` - External network for database connections (where applicable)

## Usage

Deploy an application:

```bash
cd remote/<application-name>
docker compose up -d
```

View logs:

```bash
docker compose logs -f
```

Stop an application:

```bash
docker compose down
```

## Security

- All `.env` files and `app/` folders are excluded from version control
- Database connections use internal Docker networks
- SSH tunnel required for database access
- SSL/TLS certificates managed automatically by Traefik

## Networks

- **webnet** - Shared network for all applications and Traefik
- **db** - Shared network for PostgreSQL database access
- **default** - Application-specific internal network

## Requirements

- Docker & Docker Compose
- Access to VPS for deployment
- Domain configured for `*.paweldywan.com`

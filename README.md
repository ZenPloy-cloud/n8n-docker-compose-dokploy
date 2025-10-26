# n8n for Dokploy - Standard & Queue Mode

Production-ready Docker Compose configurations for self-hosting n8n with PostgreSQL or Redis queue mode. Deploy in minutes with Dokploy's one-click setup.

## ✨ Features

- **PostgreSQL 17**: Rock-solid data persistence for your workflows
- **Task Runners**: Isolated execution for enhanced security and performance
- **Production Hardened**: Pre-configured security settings and encryption
- **Auto Cleanup**: Smart pruning of execution history (7-day retention)
- **HTTPS Ready**: SSL/TLS support with automatic certificate management
- **Queue Mode Available**: Scale horizontally with Redis-backed workers

## 📋 Prerequisites

Before you begin, make sure you have Dokploy installed on your server:

```bash
curl -sSL https://dokploy.com/install.sh | sh
```

For detailed installation instructions, visit the [Dokploy Installation Guide](https://docs.dokploy.com/docs/core/installation).

## 🚀 Quick Start with Dokploy (Standard Mode)

Follow these steps to deploy n8n in standard mode with PostgreSQL on Dokploy:

### Step 1: Create a New Project

Click on the "Create Project" button, give your project a name (e.g., "n8n-production"), and click "Create" to organize your n8n deployment.

![Create Project](./assets/dokploy-creat-project-zenploy.png)

### Step 2: Create a New Service

Click on "Create Service" to add a new service to your project. Select "Template" from the list of options.

![Create Service](./assets/dokploy-creat-service-zenploy.png)

### Step 3: Select n8n Template

In the template list, select the "n8n with Postgres" template (the second option) to get started with a pre-configured setup including PostgreSQL database.

![Select n8n Template](./assets/dokploy-n8n-templatect-zenploy.png)

### Step 4: Configure Docker Compose

Copy the contents of [`n8n-standard-postgres/docker-compose.yml`](https://github.com/ZenPloy-cloud/n8n-docker-compose-dokploy/blob/main/n8n-standard-postgres/docker-compose.yml) and paste it into the Docker Compose configuration field. Click the "Save" button at the bottom to save your configuration.

```yaml
# ---------------------------------------------------------------
# N8N Standard
# ---------------------------------------------------------------

# Docker Compose configuration file for deploying n8n with a PostgreSQL database.
# This file defines two services: 'postgres' for the database and 'n8n' for the automation application.
# It also uses Docker volumes to ensure data persistence.

services:
  postgres:
    # Uses the official PostgreSQL version 17 image, based on Alpine Linux for a smaller size.
    image: postgres:17-alpine
    # Restart policy: the container will restart automatically unless it is manually stopped.
    restart: unless-stopped
    # Environment variables for database configuration.
    # It is recommended to define these in a .env file at the root of the project for security reasons.
    environment:
      - POSTGRES_USER=${POSTGRES_USER}          # Username for the database.
      - POSTGRES_PASSWORD=${POSTGRES_PASSWORD}  # Password for the user.
      - POSTGRES_DB=${POSTGRES_DB}              # Name of the database to be created.
    # Volume for PostgreSQL data persistence.
    # 'postgres_data' is a named volume managed by Docker, mapped to the PostgreSQL data directory in the container.
    volumes:
      - postgres_data:/var/lib/postgresql/data
    # Healthcheck to ensure the database is ready to accept connections.
    healthcheck:
      # Command to check if the PostgreSQL server is ready.
      test: ["CMD-SHELL", "pg_isready -U ${POSTGRES_USER} -d ${POSTGRES_DB}"]
      # Grace period at container startup before healthcheck failures are counted.
      start_period: 30s
      # Interval between each check.
      interval: 10s
      # Maximum time allowed for the check command to run.
      timeout: 5s
      # Number of unsuccessful attempts before marking the container as 'unhealthy'.
      retries: 5

  # Service for the n8n application
  n8n:
    # Uses a specific version of the n8n image to ensure stability.
    image: n8nio/n8n:1.116.2
    # Restart policy: the container will restart automatically unless it is manually stopped.
    restart: unless-stopped
    # Makes the startup of n8n dependent on the health status of PostgreSQL.
    # n8n will only start once the 'postgres' healthcheck is successful.
    depends_on:
      postgres:
        condition: service_healthy
    # Environment variables for n8n configuration.
    environment:
      # --- Database connection configuration ---
      - DB_TYPE=postgresdb                      # Specifies the type of database to use.
      - DB_POSTGRESDB_HOST=postgres             # Database host (the name of the 'postgres' service).
      - DB_POSTGRESDB_PORT=5432                 # Database port.
      - DB_POSTGRESDB_DATABASE=${POSTGRES_DB}   # Database name (must match the one for PostgreSQL).
      - DB_POSTGRESDB_USER=${POSTGRES_USER}       # Username for the database.
      - DB_POSTGRESDB_PASSWORD=${POSTGRES_PASSWORD} # Password for the database.

      # --- Security & encryption ---
      - N8N_ENCRYPTION_KEY=${N8N_ENCRYPTION_KEY} # Secret key to encrypt sensitive data (credentials). It is very important to set and keep this safe.
      - N8N_ENFORCE_SETTINGS_FILE_PERMISSIONS=true # Enforces strict file permissions for the configuration.
      - N8N_SECURE_COOKIE=true                  # Uses secure cookies (recommended in production with HTTPS).

      # --- Host & network settings ---
      - N8N_HOST=${N8N_HOST}                    # Domain name or host on which n8n is accessible.
      - N8N_PORT=${N8N_PORT}                    # Port on which n8n listens.
      - N8N_PROTOCOL=https                      # Protocol to use (http or https).
      - NODE_ENV=production                     # Sets the Node.js execution environment to 'production' for better performance.
      - WEBHOOK_URL=https://${N8N_HOST}/         # Base URL for webhooks.
      - GENERIC_TIMEZONE=${GENERIC_TIMEZONE}    # Timezone for workflows (e.g., 'Europe/Paris').

      # --- Proxy Configuration ---
      # Useful if n8n is behind a reverse proxy (like Nginx or Traefik).
      - N8N_TRUST_PROXY=true                    # Tells n8n to trust X-Forwarded-* headers.
      - N8N_PROXY_HOPS=1                        # Number of proxies between the client and the server.

      # --- Performance, cleanup & future compatibility ---
      - EXECUTIONS_DATA_PRUNE=true              # Enables automatic pruning of past execution data.
      - EXECUTIONS_DATA_MAX_AGE=168             # Maximum age (in hours) of execution data to keep (here, 7 days).
      - N8N_PERSONALIZATION_ENABLED=false       # Disables the collection of personalization data.
      - N8N_TEMPLATES_ENABLED=true              # Enables access to workflow templates.
      - N8N_RUNNERS_ENABLED=true                # Enables task runners for isolated and secure workflow execution (recommended).
      - N8N_BLOCK_ENV_ACCESS_IN_NODE=false      # Allows access to environment variables in code nodes (use with caution).
      - N8N_GIT_NODE_DISABLE_BARE_REPOS=true    # Security setting for the Git node.

    # Volume for n8n data persistence (workflows, credentials, etc.).
    # 'n8n_data' is a named volume that will be mapped to the n8n configuration directory.
    volumes:
      - n8n_data:/home/node/.n8n
    # Removed the 'deploy' section as requested to avoid system resource limits.
    # This makes the setup more flexible, especially for development or non-swarm environments.

# Definition of named volumes to ensure data persistence
# even if the containers are removed and recreated.
volumes:
  n8n_data:
  postgres_data:
```

![Deploy Docker Compose](./assets/dokploy-deploy-docker-compose-zenploy.png)

### Step 5: Set Environment Variables

Go to the "Environment" tab and configure the required environment variables. Use the template from [`n8n-standard-postgres/dockploy-environment-settings.env`](https://github.com/ZenPloy-cloud/n8n-docker-compose-dokploy/blob/main/n8n-standard-postgres/dockploy-environment-settings.env):

```bash
# ---------------------------------------------------------------
# N8N Settings
# ---------------------------------------------------------------

# The full domain name where your n8n instance will be accessible.
# Example: n8n.your-domain.com
N8N_HOST=n8n.example.com

# The port on which n8n will listen. The default is 5678.
N8N_PORT=5678

# The timezone for scheduling workflows.
# A list of valid timezones can be found here: https://en.wikipedia.org/wiki/List_of_tz_database_time_zones
# Example: Europe/Paris, America/New_York
GENERIC_TIMEZONE=Europe/Paris

# The encryption key for credentials.
# You can generate one with the following command: openssl rand -base64 32
N8N_ENCRYPTION_KEY=CHANGEME_GENERATE_A_RANDOM_32_CHAR_KEY

# ---------------------------------------------------------------
# PostgreSQL Database Settings
# ---------------------------------------------------------------

# The username to connect to the PostgreSQL database.
# Avoid using "root" or "admin". A good choice is "n8n".
POSTGRES_USER=n8n

# The password for the PostgreSQL user.
# You can generate one with the following command: openssl rand -base64 64
POSTGRES_PASSWORD=CHANGEME_SET_A_VERY_SECURE_PASSWORD

# The name of the database that n8n will use.
POSTGRES_DB=n8n
```

After entering all variables, click the "Save" button to save your configuration.

![Environment Settings](./assets/dokploy-environment-settings-zenploy.png)

### Step 6: Deploy the Application

Go back to the "General" tab and click the "Deploy" button to start the initial deployment.

![Deploy](./assets/dokploy-deploy-zenploy.png)

### Step 7: Check Deployment Details

Monitor the deployment logs to ensure everything is running correctly.

![Deployment Details](./assets/dokploy-deployment-details-zenploy.png)

### Step 8: Configure Domain

Go to the Domains tab.

    Modify the default domain to match your N8N_HOST value (e.g., n8n.yourdomain.com).

    Enable HTTPS and select Let's Encrypt for automatic SSL.

    Click Update.

![Domain Configuration](./assets/dokploy-domains-zenploy.png)

### Step 9: Deploy Again

After updating the domain, click Deploy one more time to apply the changes.

![Deploy](./assets/dokploy-deploy-zenploy.png)

### Step 10: Set Up n8n Owner Account

Access your n8n instance at your configured domain and create the owner account. You're all set!

![Set Up Owner Account](./assets/n8n-set-up-owner-account.png)

## Configuration Details

### Docker Compose Structure

The setup includes two services:

- **postgres**: PostgreSQL 17 database with health checks
- **n8n**: n8n application (version 1.116.2) with task runners enabled

### Security Features

- Encrypted credentials with `N8N_ENCRYPTION_KEY`
- Secure cookies enabled
- Strict file permissions enforcement
- Git node bare repository protection
- Reverse proxy trust configuration

### Data Persistence

Two Docker volumes ensure data persistence:
- `postgres_data`: Database storage
- `n8n_data`: n8n workflows and credentials

### Automatic Cleanup

Execution data older than 7 days (168 hours) is automatically pruned to save storage space.

## Support

For issues and questions:
- [n8n Documentation](https://docs.n8n.io/)
- [n8n Community Forum](https://community.n8n.io/)
- [Dokploy Documentation](https://dokploy.com/docs)

## License

This configuration is provided as-is under the MIT License. n8n itself is licensed under the [Sustainable Use License](https://github.com/n8n-io/n8n/blob/master/LICENSE.md).

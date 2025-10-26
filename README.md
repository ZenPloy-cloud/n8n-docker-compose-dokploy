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

Copy the contents of [`n8n-standard-with-postgres/docker-compose.yml`](https://github.com/ZenPloy-cloud/n8n-docker-compose-dokploy/blob/main/n8n-standard-postgres/docker-compose.yml) from this repository and paste it into the Docker Compose configuration field. Click the "Save" button at the bottom to save your configuration.

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

Go to the "Domains" tab and modify the default domain name to match your `N8N_HOST` value. Enable HTTPS by clicking on the HTTPS toggle and select "Let's Encrypt" for automatic SSL certificate generation. Click the "Update" button to save your domain configuration.

![Domain Configuration](./assets/dokploy-domains-zenploy.png)

### Step 9: Deploy Again

After configuring the domain, deploy again to apply the changes.

![Deploy](./assets/dokploy-deploy-zenploy.png)

### Step 10: Set Up n8n Owner Account

Access your n8n instance at your configured domain and create the owner account.

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

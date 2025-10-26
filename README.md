# n8n on Dokploy - Production Ready
### Deploy Standard or Queue Mode in Minutes

Production-ready Docker Compose configurations for self-hosting n8n with PostgreSQL or Redis queue mode. One-click deployment with Dokploy.

<div align="center">

![Docker](https://img.shields.io/badge/Docker-Ready-2496ED?style=flat-square&logo=docker&logoColor=white)
![n8n](https://img.shields.io/badge/n8n-Automation-EA4B71?style=flat-square&logo=n8n&logoColor=white)
![Redis](https://img.shields.io/badge/Redis-Queue-DC382D?style=flat-square&logo=redis&logoColor=white)
![PostgreSQL](https://img.shields.io/badge/PostgreSQL-Database-4169E1?style=flat-square&logo=postgresql&logoColor=white)

</div>

## ✨ Features

### Standard Mode
- **PostgreSQL 17**: Rock-solid data persistence for your workflows
- **Task Runners**: Isolated execution for enhanced security and performance
- **Production Hardened**: Pre-configured security settings and encryption
- **Auto Cleanup**: Smart pruning of execution history (7-day retention)
- **HTTPS Ready**: SSL/TLS support with automatic certificate management
- **Simple Setup**: Perfect for most use cases with single-instance deployment

### Queue Mode (Advanced)
- **Redis Message Broker**: Distributed job queue for high-volume processing
- **Horizontal Scaling**: Add/remove workers dynamically based on workload
- **Multiple Workers**: Run parallel workflow executions across worker instances
- **High Availability**: Separate UI/webhooks from workflow execution
- **Worker Concurrency**: Each worker handles 10 parallel executions by default
- **Production Grade**: Built for enterprise-level workflow automation

## 📋 Prerequisites

Before you begin, make sure you have Dokploy installed on your server:

```bash
curl -sSL https://dokploy.com/install.sh | sh
```

For detailed installation instructions, visit the [Dokploy Installation Guide](https://docs.dokploy.com/docs/core/installation).

## 🚀 Quick Start with Dokploy (Standard Mode)

Follow these steps to deploy n8n in standard mode with PostgreSQL on Dokploy:

### Step 1: Create a New Project

In Dokploy, navigate to **Projects** and click **Create Project**. Name it n8n-standard-production and click **Create**.

![Create Project](./assets/dokploy-creat-project-zenploy.png)

### Step 2: Create a New Service

Click on **Create Service** to add a new service to your project. Select **Template** from the list of options.

![Create Service](./assets/dokploy-creat-service-zenploy.png)

### Step 3: Select n8n Template

In the template list, select the "n8n with Postgres" template (the second option) to get started with a pre-configured setup including PostgreSQL database. Click **Create** to proceed.

![Select n8n Template](./assets/dokploy-n8n-templatect-zenploy.png)

### Step 4: Configure Docker Compose

Copy the contents of [`n8n-standard-postgres/docker-compose.yml`](https://github.com/ZenPloy-cloud/n8n-docker-compose-dokploy/blob/main/n8n-standard-postgres/docker-compose.yml) and paste it into the **Compose File** field. Click **Save** at the bottom to save your configuration.

![Deploy Docker Compose](./assets/dokploy-deploy-docker-compose-zenploy.png)

### Step 5: Set Environment Variables

Go to the **Environment** tab and copy the template from [`n8n-standard-postgres/dockploy-environment-settings.env`](https://github.com/ZenPloy-cloud/n8n-docker-compose-dokploy/blob/main/n8n-standard-postgres/dockploy-environment-settings.env) into the **Environment Settings** field:

![Environment Settings](./assets/dokploy-environment-settings-zenploy.png)

After entering all variables, click the **Save** button to save your configuration.

### Step 6: Deploy the Application

Go back to the **General** tab and click the **Deploy** button to start the initial deployment.

![Deploy](./assets/dokploy-deploy-zenploy.png)

### Step 7: Check Deployment Details

Monitor the deployment logs to ensure everything is running correctly.

![Deployment Details](./assets/dokploy-deployment-details-zenploy.png)

### Step 8: Configure Domain

1. Go to the **Domains** tab
2. Modify the default domain to match your `N8N_HOST` value (e.g., n8n.yourdomain.com)
3. Enable HTTPS and select **Let's Encrypt** for automatic SSL
4. Click **Update**

![Domain Configuration](./assets/dokploy-domains-zenploy.png)

### Step 9: Deploy Again

After updating the domain, click **Deploy** one more time to apply the changes.

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

---

## 🚀 Queue Mode (For High-Volume Workflows)

Need to run hundreds of workflows simultaneously? Queue mode is for you.

### 💡 Do You Need Queue Mode?

**Choose Queue Mode if:**
- ✅ You run 100+ workflows per hour
- ✅ Your workflows are getting delayed or timing out
- ✅ You need to scale up during peak hours
- ✅ You want to separate your UI from workflow execution

**Stick with Standard Mode if:**
- ❌ You run less than 50 workflows per hour
- ❌ You're just getting started with n8n
- ❌ You want the simplest setup possible

### 🏗️ How It Works

Think of it like a restaurant kitchen:

```
┌─────────────┐     ┌─────────┐     ┌──────────┐
│   n8n Main  │────▶│  Redis  │────▶│ Worker 1 │ 👨‍🍳
│  (Waiter)   │     │ (Orders)│     └──────────┘
└─────────────┘     └─────────┘     ┌──────────┐
                                    │ Worker 2 │ 👨‍🍳
                                    └──────────┘
```

- **Main instance** = Waiter (takes orders, serves customers)
- **Redis** = Order tickets (queues up the work)
- **Workers** = Chefs (do the actual cooking)

### 🎯 Deploy Queue Mode in Dokploy

**Step 1:** Follow the same deployment steps as Standard Mode (Steps 1-10 above)

**Step 2:** Use these files instead:
- 📄 Docker Compose: [`n8n-queue-mode-redis/docker-compose.yml`](https://github.com/ZenPloy-cloud/n8n-docker-compose-dokploy/blob/main/n8n-queue-mode-redis/docker-compose.yml)
- ⚙️ Environment: [`n8n-queue-mode-redis/dockploy-environment-settings.env`](https://github.com/ZenPloy-cloud/n8n-docker-compose-dokploy/blob/main/n8n-queue-mode-redis/dockploy-environment-settings.env)

**That's it!** Your queue mode setup includes:
- 🗄️ PostgreSQL (database)
- 🔴 Redis (message queue)
- 🖥️ n8n Main (UI & webhooks)
- ⚙️ 1 Worker (processes workflows)

### Scaling Your Automation

**Add More Workers**

When workflows start queuing up, just add more workers to `docker-compose.yml`:

```yaml
# Add this to your docker-compose.yml services section
n8n-worker-2:
  image: n8nio/n8n:1.116.2
  restart: unless-stopped
  command: worker --concurrency=10
  depends_on:
    postgres:
      condition: service_healthy
    redis:
      condition: service_healthy
  environment:
    - EXECUTIONS_MODE=queue
    - DB_TYPE=postgresdb
    - DB_POSTGRESDB_HOST=postgres
    - DB_POSTGRESDB_PORT=5432
    - DB_POSTGRESDB_DATABASE=${POSTGRES_DB}
    - DB_POSTGRESDB_USER=${POSTGRES_USER}
    - DB_POSTGRESDB_PASSWORD=${POSTGRES_PASSWORD}
    - QUEUE_BULL_REDIS_HOST=redis
    - QUEUE_BULL_REDIS_PORT=6379
    - QUEUE_BULL_REDIS_DB=0
    - QUEUE_BULL_REDIS_TIMEOUT_THRESHOLD=10000
    - N8N_GRACEFUL_SHUTDOWN_TIMEOUT=30
    - N8N_ENCRYPTION_KEY=${N8N_ENCRYPTION_KEY}
    - GENERIC_TIMEZONE=${GENERIC_TIMEZONE}
    - N8N_RUNNERS_ENABLED=true
  volumes:
    - n8n_data:/home/node/.n8n
```

Then deploy to apply changes:
1. Go to the **General** tab in Dokploy
2. Click **Deploy** to restart with the new worker

**Concurrency Recommendations:**
- Set concurrency to **5-10** per worker
- Avoid low concurrency with many workers (exhausts database connections)
- Monitor worker performance and adjust accordingly

### Important Notes

⚠️ **Encryption Key**: All workers must use the **same** `N8N_ENCRYPTION_KEY` as the main instance to access credentials.

⚠️ **Binary Data**: If workflows use binary data, configure S3 external storage instead of filesystem.

### Monitoring Workers

Workers expose health check endpoints (when `QUEUE_HEALTH_CHECK_ACTIVE` is enabled):
- `/healthz`: Worker status
- `/healthz/readiness`: DB and Redis connection status
- `/metrics`: Performance metrics

---

## Support

For issues and questions:
- [n8n Documentation](https://docs.n8n.io/)
- [n8n Community Forum](https://community.n8n.io/)
- [Dokploy Documentation](https://dokploy.com/docs)
- [Dokploy Documentation](https://dokploy.com/docs)
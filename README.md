# n8n on Dokploy - Production Ready
### Deploy Standard or Queue Mode in Minutes

Production-ready Docker Compose configurations for self-hosting n8n with PostgreSQL or Redis queue mode. One-click deployment with Dokploy.

<div align="center">

![Docker](https://img.shields.io/badge/Docker-Ready-2496ED?style=flat-square&logo=docker&logoColor=white)
![n8n](https://img.shields.io/badge/n8n-Automation-EA4B71?style=flat-square&logo=n8n&logoColor=white)
![Redis](https://img.shields.io/badge/Redis-Queue-DC382D?style=flat-square&logo=redis&logoColor=white)
![PostgreSQL](https://img.shields.io/badge/PostgreSQL-Database-4169E1?style=flat-square&logo=postgresql&logoColor=white)

</div>

---

## 🏗️ Architecture Overview

<div align="center">

<table>
<tr>
<td width="50%" align="center">

### 📦 Standard Mode
**Perfect for getting started**

```
     ┌─────────────────────┐
     │   n8n Instance      │
     │  ┌───────────────┐  │
     │  │  UI & Editor  │  │
     │  ├───────────────┤  │
     │  │   Workflows   │  │
     │  ├───────────────┤  │
     │  │   Webhooks    │  │
     │  └───────────────┘  │
     └──────────┬──────────┘
                │
                ▼
     ┌─────────────────────┐
     │   PostgreSQL 17     │
     │   🐘 Database       │
     └─────────────────────┘
```

**Components:**
- 1 n8n instance (all-in-one)
- 1 PostgreSQL database
- Simple & reliable

</td>
<td width="50%" align="center">

### ⚡ Queue Mode
**Built for scale**

```
┌──────────────┐
│  n8n Main    │
│  ┌────────┐  │
│  │   UI   │  │
│  └────────┘  │
└──────┬───────┘
       │
       ▼
┌──────────────┐      ┌─────────────┐
│    Redis     │─────▶│  Worker 1   │
│  📮 Queue    │      │  ⚙️ Executor │
└──────┬───────┘      └─────────────┘
       │              ┌─────────────┐
       └─────────────▶│  Worker 2   │
       │              │  ⚙️ Executor │
       │              └─────────────┘
       ▼              ┌─────────────┐
┌──────────────┐     │  Worker N   │
│ PostgreSQL   │◀────│  ⚙️ Executor │
│ 🐘 Database  │     └─────────────┘
└──────────────┘
```

**Components:**
- 1 main instance (UI/webhooks)
- 1 Redis message broker
- N workers (scalable)
- 1 PostgreSQL database

</td>
</tr>
</table>

</div>

---

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

## 🚀 Deploy n8n in 10 Minutes (Standard Mode)

Perfect for most use cases. Let's get you up and running!

### Step 1: Create Your Project

Click **Create Project** in Dokploy, give it a name (like "n8n-production"), and hit **Create**.

![Create Project](./assets/dokploy-creat-project-zenploy.png)

### Step 2: Add a Service

Click **Create Service** → Select **Template** from the options.

![Create Service](./assets/dokploy-creat-service-zenploy.png)

### Step 3: Pick the n8n Template

Choose **"n8n with Postgres"** (the 2nd option) → Click **Create**.

![Select n8n Template](./assets/dokploy-n8n-templatect-zenploy.png)

### Step 4: Paste the Docker Config

Copy [`this docker-compose.yml`](https://github.com/ZenPloy-cloud/n8n-docker-compose-dokploy/blob/main/n8n-standard-postgres/docker-compose.yml) → Paste into **Compose File** → Click **Save**.

![Deploy Docker Compose](./assets/dokploy-deploy-docker-compose-zenploy.png)

### Step 5: Set Your Environment Variables

Go to **Environment** tab → Copy [`these settings`](https://github.com/ZenPloy-cloud/n8n-docker-compose-dokploy/blob/main/n8n-standard-postgres/dockploy-environment-settings.env) → Paste into **Environment Settings**.

**Important:** Change these values:
- `N8N_HOST` → Your domain (e.g., n8n.yourdomain.com)
- `N8N_ENCRYPTION_KEY` → Generate with: `openssl rand -base64 32`
- `POSTGRES_PASSWORD` → Generate with: `openssl rand -base64 64`

Click **Save** when done.

![Environment Settings](./assets/dokploy-environment-settings-zenploy.png)

### Step 6: First Deploy

Go to **General** tab → Click **Deploy** to start.

![Deploy](./assets/dokploy-deploy-zenploy.png)

### Step 7: Watch It Build

Check the logs to make sure everything's working. You'll see PostgreSQL and n8n starting up.

![Deployment Details](./assets/dokploy-deployment-details-zenploy.png)

### Step 8: Set Up Your Domain

1. Go to **Domains** tab
2. Change the domain to match your `N8N_HOST` (e.g., n8n.yourdomain.com)
3. Toggle **HTTPS** on → Select **Let's Encrypt**
4. Click **Update**

![Domain Configuration](./assets/dokploy-domains-zenploy.png)

### Step 9: Deploy Again (Final Time!)

Go back to **General** → Click **Deploy** to apply the domain changes.

![Deploy](./assets/dokploy-deploy-zenploy.png)

### Step 10: Create Your Account 🎉

Visit your domain (e.g., https://n8n.yourdomain.com) and create your owner account. You're live!

![Set Up Owner Account](./assets/n8n-set-up-owner-account.png)

## 🔧 What's Under the Hood?

### 📦 What Gets Installed

Your n8n setup includes:
- **PostgreSQL 17** - Your workflow database (with automatic health checks)
- **n8n 1.116.2** - The automation engine (with task runners for better security)

### 🔒 Security Built-In

We've got you covered:
- 🔐 **Encrypted Credentials** - All your API keys and passwords are encrypted with `N8N_ENCRYPTION_KEY`
- 🍪 **Secure Cookies** - HTTPS-only cookies for your sessions
- 📁 **Protected Files** - Strict permissions on configuration files
- 🛡️ **Git Protection** - Prevents bare repository exploits
- 🔄 **Proxy Ready** - Works seamlessly behind reverse proxies

### 💾 Your Data is Safe

Everything important is stored in Docker volumes:
- `postgres_data` - All your workflow definitions and execution history
- `n8n_data` - Your credentials, settings, and configurations

**Even if you delete the containers, your data stays safe!**

### 🧹 Auto-Cleanup

To keep things tidy, n8n automatically deletes execution logs older than 7 days. Your workflows and credentials are never touched - only the execution history gets cleaned up.

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

### 📈 Need More Power? Add Workers!

Workflows piling up? Just add more workers to handle the load.

**How to add a second worker:**

1. Open your `docker-compose.yml` in Dokploy
2. Copy-paste this at the end of the `services:` section:

```yaml
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

3. Click **Save**
4. Go to **General** tab → Click **Deploy**

**Pro Tips:**
- 🎯 Each worker can handle 10 workflows at once (that's the `--concurrency=10`)
- 🚀 Start with 1-2 workers, add more if needed
- ⚡ Need a third worker? Copy the code again and rename it to `n8n-worker-3`

### ⚠️ Important Things to Know

**Same Encryption Key Everywhere**
All workers MUST use the same `N8N_ENCRYPTION_KEY`. Otherwise, they can't access your saved credentials. Think of it like a master key that unlocks all your workflow secrets.

**Binary Files & Queue Mode**
If your workflows handle files (images, PDFs, etc.), you'll need to set up S3 storage. The default file storage doesn't work well with multiple workers.

### 🔍 Monitor Your Workers (Optional)

Want to check if your workers are healthy? Enable health checks by adding this to your environment variables:

```bash
QUEUE_HEALTH_CHECK_ACTIVE=true
```

Then you can check:
- `http://your-worker:5678/healthz` - Is the worker running?
- `http://your-worker:5678/healthz/readiness` - Can it connect to DB and Redis?
- `http://your-worker:5678/metrics` - Performance stats

---

## Support

For issues and questions:
- [n8n Documentation](https://docs.n8n.io/)
- [n8n Community Forum](https://community.n8n.io/)
- [Dokploy Documentation](https://dokploy.com/docs)
- [Dokploy Documentation](https://dokploy.com/docs)
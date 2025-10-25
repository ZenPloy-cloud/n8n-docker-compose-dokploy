# n8n Production Stack for Docker & Dokploy

This repository provides production-ready Docker Compose configurations to self-host the n8n automation platform. It is designed for easy deployment using Dokploy or any standard Docker environment.

## Choose Your Deployment Mode

This repository offers two deployment modes. Choose the one that best fits your needs.

### 1. Standard Mode

A simple and efficient setup with a single n8n instance connected to a PostgreSQL database. Perfect for most use cases and easy to manage.

#### Step 1: Create a New Project
![Create Project](./assets/dokploy-creat-project-zenploy.png)

#### Step 2: Create a New Service
![Create Service](./assets/dokploy-creat-service-zenploy.png)

#### Step 3: Select n8n Template
![Select n8n Template](./assets/dokploy-n8n-templatect-zenploy.png)

#### Step 4: Configure Docker Compose
![Deploy Docker Compose](./assets/dokploy-deploy-docker-compose-zenploy.png)

#### Step 5: Set Environment Variables
![Environment Settings](./assets/dokploy-environment-settings-zenploy.png)

#### Step 6: Deploy the Application
![Deploy](./assets/dokploy-deploy-zenploy.png)

#### Step 7: Configure Domain
![Domain Configuration](./assets/dokploy-domains-zenploy.png)

#### Step 8: Check Deployment Details
![Deployment Details](./assets/dokploy-deployment-details-zenploy.png)

#### Step 9: Deploy Again (After Configuration)
![Deploy](./assets/dokploy-deploy-zenploy.png)

#### Step 10: Set Up n8n Owner Account
![Set Up Owner Account](./n8n-set-up-owner-account.png)

**[>> Go to Standard Mode instructions](./standard/)**

### 2. Queue Mode (Recommended for High Volume)

A scalable setup using Redis to manage a queue of executions, which are processed by dedicated worker services. This mode is ideal for high-volume workflows or to ensure the main n8n instance remains responsive.

**[>> Go to Queue Mode instructions](./queue/)**

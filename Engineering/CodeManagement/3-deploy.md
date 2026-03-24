# Deployment Guide

## Docker Deployment Architecture

### Overview

We use a **Docker Primary / Docker Secondary** architecture for our deployments. This is a **multi-site cluster deployment strategy** where each site (Primary and Secondary) represents a complete set of containerized applications running in a physical datacenter.

**Key Benefits:**

- **High Availability** — Geographic redundancy across datacenters
- **Disaster Recovery** — Automatic failover to Secondary site
- **Zero-Downtime Deployments** — Blue-green deployments at the cluster level
- **Easy Rollback** — Simply switch traffic back to the previous site

---

## Docker Primary & Docker Secondary Concept

### Overview

Docker Primary and Docker Secondary are **not individual containers** — they represent **complete application clusters** deployed in separate physical sites (datacenters). This architecture provides geographic redundancy, disaster recovery capabilities, and enables seamless blue-green deployments at the infrastructure level.

---

### What is Docker Primary?

**Docker Primary** is the main cluster of applications running in the **primary datacenter/site**. It is the active environment that serves all production traffic.

A Docker Primary cluster typically includes:

- **Applications** (API, Web, Workers)
- **Databases** (PostgreSQL, MongoDB, Redis)
- **Message Queues** (RabbitMQ, Kafka)
- **Caching Layers** (Redis, Memcached)
- **Reverse Proxy / Load Balancer** (Nginx, Traefik)
- **Monitoring & Logging** (Prometheus, Grafana, ELK)

**Characteristics:**

- Located in the **primary datacenter** (e.g., DC1, Site A)
- Receives all production traffic via DNS/Load Balancer
- Contains the current stable version of all applications
- Maintains primary database with real-time data
- Runs on dedicated infrastructure with full resources

---

### What is Docker Secondary?

**Docker Secondary** is the standby cluster of applications running in a **secondary datacenter/site**. It mirrors the primary environment and serves as a failover target and staging ground for new deployments.

A Docker Secondary cluster contains the same applications as Primary:

- **Applications** (mirrored from Primary)
- **Databases** (replicated or synced from Primary)
- **Message Queues** (configured for failover)
- **Caching Layers** (independent or synced)
- **Reverse Proxy / Load Balancer** (ready to receive traffic)
- **Monitoring & Logging** (independent stack)

**Characteristics:**

- Located in a **secondary datacenter** (e.g., DC2, Site B)
- Standby mode — does not receive production traffic until switched
- Used for staging new releases before promoting to Primary
- Database replication from Primary (async or sync)
- Enables zero-downtime deployments and disaster recovery

---

### Architecture Diagram

```
                        ┌─────────────────────────────┐
                        │      Global DNS / CDN       │
                        │    (Cloudflare, Route53)    │
                        └─────────────┬───────────────┘
                                      │
                                      ▼
                        ┌─────────────────────────────┐
                        │   Global Load Balancer      │
                        │      (Active/Standby)       │
                        └─────────────┬───────────────┘
                                      │
            ┌─────────────────────────┴─────────────────────────┐
            │                                                   │
            ▼                                                   ▼
┌───────────────────────────────────┐       ┌───────────────────────────────────┐
│        DOCKER PRIMARY             │       │        DOCKER SECONDARY           │
│     (Datacenter 1 / Site A)       │       │     (Datacenter 2 / Site B)       │
│           [ACTIVE]                │       │          [STANDBY]                │
├───────────────────────────────────┤       ├───────────────────────────────────┤
│                                   │       │                                   │
│  ┌─────────┐  ┌─────────┐        │       │  ┌─────────┐  ┌─────────┐        │
│  │   API   │  │   Web   │        │       │  │   API   │  │   Web   │        │
│  │   App   │  │   App   │        │       │  │   App   │  │   App   │        │
│  └─────────┘  └─────────┘        │       │  └─────────┘  └─────────┘        │
│                                   │       │                                   │
│  ┌─────────┐  ┌─────────┐        │       │  ┌─────────┐  ┌─────────┐        │
│  │ Worker  │  │  Redis  │        │       │  │ Worker  │  │  Redis  │        │
│  │   App   │  │  Cache  │        │       │  │   App   │  │  Cache  │        │
│  └─────────┘  └─────────┘        │       │  └─────────┘  └─────────┘        │
│                                   │       │                                   │
│  ┌─────────────────────┐         │       │  ┌─────────────────────┐         │
│  │     PostgreSQL      │─────────┼──────▶│  │ PostgreSQL (Replica)│         │
│  │      (Primary)      │  Replication    │  │     (Standby)       │         │
│  └─────────────────────┘         │       │  └─────────────────────┘         │
│                                   │       │                                   │
│  Version: v1.2.0                 │       │  Version: v1.3.0 (staged)        │
└───────────────────────────────────┘       └───────────────────────────────────┘
```

---

### Site Responsibilities

| Aspect          | Docker Primary (Site A)          | Docker Secondary (Site B)           |
| --------------- | -------------------------------- | ----------------------------------- |
| **Traffic**     | Serves 100% production traffic   | Standby (0% traffic until switch)   |
| **Database**    | Primary (read/write)             | Replica (read-only until promotion) |
| **Deployments** | Receives updates after Secondary | Staging ground for new releases     |
| **Purpose**     | Production workloads             | Failover + Blue-green staging       |
| **Monitoring**  | Full production monitoring       | Health monitoring + alerts          |

---

### Deployment & Switching Strategy

#### Standard Deployment Flow (Zero-Downtime)

1. **Deploy to Secondary** — New version is deployed to the Secondary cluster
2. **Test & Validate** — Run smoke tests, health checks, and QA on Secondary
3. **Switch Traffic** — Update DNS/Load Balancer to route traffic to Secondary
4. **Secondary becomes Primary** — The roles are now swapped
5. **Update old Primary** — Deploy the same version to the former Primary (now Secondary)

#### Failover Scenario

1. Primary site experiences outage or degradation
2. Health checks detect failure
3. Automatic or manual switch to Secondary
4. Secondary promotes database replica to primary
5. Traffic routed to Secondary site

#### Rollback Strategy

1. If issues detected after switch, simply revert DNS/Load Balancer
2. Traffic flows back to the previous Primary
3. No redeployment needed — both versions are running

---

## Directory Structure

Each site (Primary/Secondary) maintains the following directory structure on its deployment server:

```
/home/deploy/{project_name}/
├── docker-compose.yml              # Base compose with shared app definitions
├── docker-compose.primary.yml      # Primary site overrides
├── docker-compose.secondary.yml    # Secondary site overrides
├── .env                            # Active environment variables
├── .env.primary                    # Primary site configuration
├── .env.secondary                  # Secondary site configuration
├── clone.sh                        # Repository clone/update script
├── deploy.sh                       # App deployment script
├── switch.sh                       # Traffic switching script
├── rollback.sh                     # Rollback automation
├── apps/                           # Application-specific configurations
│   ├── api/
│   ├── web/
│   ├── worker/
│   └── ...
└── logs/
    └── deploy.log                  # Deployment history
```

---

## Clone Script

The clone script is responsible for pulling the latest code from the repository and setting up the environment.

### `clone.sh` example

```bash
#!/bin/bash

#######################################
# Clone Script for Project Deployment
# Usage: ./clone.sh <branch> [environment]
#######################################

set -e

# Configuration
REPO_URL="${REPO_URL:-git@github.com:snet-co/{PROJECT_NAME}.git}"
DEPLOY_PATH="${DEPLOY_PATH:-/home/deploy/{PROJECT_NAME}}"
BRANCH="${1:-main}"
ENVIRONMENT="${2:-production}"

# Colors for output
RED='\033[0;31m'
GREEN='\033[0;32m'
YELLOW='\033[1;33m'
NC='\033[0m' # No Color

echo -e "${GREEN}========================================${NC}"
echo -e "${GREEN}  Clone Script - ${ENVIRONMENT}${NC}"
echo -e "${GREEN}========================================${NC}"

# Create deploy directory if not exists
if [ ! -d "$DEPLOY_PATH" ]; then
    echo -e "${YELLOW}Creating deployment directory: ${DEPLOY_PATH}${NC}"
    mkdir -p "$DEPLOY_PATH"
fi

cd "$DEPLOY_PATH"

# Check if repo exists
if [ -d ".git" ]; then
    echo -e "${YELLOW}Repository exists. Fetching updates...${NC}"
    git fetch --all
    git checkout "$BRANCH"
    git pull origin "$BRANCH"
else
    echo -e "${YELLOW}Cloning repository...${NC}"
    git clone -b "$BRANCH" "$REPO_URL" .
fi

# Copy environment file if exists
if [ -f ".env.${ENVIRONMENT}" ]; then
    echo -e "${YELLOW}Copying environment file...${NC}"
    cp ".env.${ENVIRONMENT}" .env
fi

echo -e "${GREEN}Clone completed successfully!${NC}"
echo -e "Branch: ${BRANCH}"
echo -e "Path: ${DEPLOY_PATH}"
echo -e "Commit: $(git rev-parse --short HEAD)"
```

---

## Deploy Script

The deploy script handles building and deploying Docker applications.

### `deploy.sh` example

```bash
#!/bin/bash

#######################################
# Deploy Script for Docker Primary/Secondary
# Usage: ./deploy.sh <primary|secondary> [--force]
#######################################

set -e

# Configuration
DEPLOY_TARGET="${1:-primary}"
FORCE_DEPLOY="${2:-}"
DEPLOY_PATH="${DEPLOY_PATH:-/home/deploy/{PROJECT_NAME}}"
LOG_FILE="${DEPLOY_PATH}/logs/deploy.log"

# Docker compose files
COMPOSE_BASE="docker-compose.yml"
COMPOSE_PRIMARY="docker-compose.primary.yml"
COMPOSE_SECONDARY="docker-compose.secondary.yml"

# Container naming
CONTAINER_PRIMARY="{project_name}_primary"
CONTAINER_SECONDARY="{project_name}_secondary"

# Colors
RED='\033[0;31m'
GREEN='\033[0;32m'
YELLOW='\033[1;33m'
BLUE='\033[0;34m'
NC='\033[0m'

# Logging function
log() {
    local timestamp=$(date '+%Y-%m-%d %H:%M:%S')
    echo -e "[$timestamp] $1" | tee -a "$LOG_FILE"
}

echo -e "${BLUE}========================================${NC}"
echo -e "${BLUE}  Docker Deploy - ${DEPLOY_TARGET}${NC}"
echo -e "${BLUE}========================================${NC}"

cd "$DEPLOY_PATH"

# Create logs directory
mkdir -p logs

log "${GREEN}Starting deployment to ${DEPLOY_TARGET}...${NC}"

# Determine which compose file to use
if [ "$DEPLOY_TARGET" == "primary" ]; then
    COMPOSE_FILE="$COMPOSE_PRIMARY"
    CONTAINER_NAME="$CONTAINER_PRIMARY"
    ENV_FILE=".env.primary"
elif [ "$DEPLOY_TARGET" == "secondary" ]; then
    COMPOSE_FILE="$COMPOSE_SECONDARY"
    CONTAINER_NAME="$CONTAINER_SECONDARY"
    ENV_FILE=".env.secondary"
else
    log "${RED}Invalid target. Use 'primary' or 'secondary'${NC}"
    exit 1
fi

# Check if compose file exists
if [ ! -f "$COMPOSE_FILE" ]; then
    log "${RED}Compose file not found: ${COMPOSE_FILE}${NC}"
    exit 1
fi

# Load environment variables
if [ -f "$ENV_FILE" ]; then
    export $(cat "$ENV_FILE" | grep -v '^#' | xargs)
    log "${YELLOW}Loaded environment from ${ENV_FILE}${NC}"
fi

# Pull latest images (if using registry)
log "${YELLOW}Pulling latest images...${NC}"
docker-compose -f "$COMPOSE_BASE" -f "$COMPOSE_FILE" pull || true

# Build images
log "${YELLOW}Building Docker images...${NC}"
docker-compose -f "$COMPOSE_BASE" -f "$COMPOSE_FILE" build --no-cache

# Stop existing container if force deploy
if [ "$FORCE_DEPLOY" == "--force" ]; then
    log "${YELLOW}Force stopping existing container...${NC}"
    docker-compose -f "$COMPOSE_BASE" -f "$COMPOSE_FILE" down || true
fi

# Start new container
log "${YELLOW}Starting new container...${NC}"
docker-compose -f "$COMPOSE_BASE" -f "$COMPOSE_FILE" up -d

# Health check
log "${YELLOW}Running health check...${NC}"
HEALTH_CHECK_URL="${HEALTH_CHECK_URL:-http://localhost:3000/health}"

if [ "$DEPLOY_TARGET" == "secondary" ]; then
    HEALTH_CHECK_URL="${HEALTH_CHECK_URL_SECONDARY:-http://localhost:3001/health}"
fi

MAX_RETRIES=30
RETRY_COUNT=0

while [ $RETRY_COUNT -lt $MAX_RETRIES ]; do
    if curl -sf "$HEALTH_CHECK_URL" > /dev/null 2>&1; then
        log "${GREEN}Health check passed!${NC}"
        break
    fi
    RETRY_COUNT=$((RETRY_COUNT + 1))
    log "${YELLOW}Waiting for service to be ready... (${RETRY_COUNT}/${MAX_RETRIES})${NC}"
    sleep 2
done

if [ $RETRY_COUNT -eq $MAX_RETRIES ]; then
    log "${RED}Health check failed after ${MAX_RETRIES} attempts${NC}"
    log "${RED}Rolling back...${NC}"
    docker-compose -f "$COMPOSE_BASE" -f "$COMPOSE_FILE" down
    exit 1
fi

# Show deployment info
log "${GREEN}========================================${NC}"
log "${GREEN}  Deployment Successful!${NC}"
log "${GREEN}========================================${NC}"
log "Target: ${DEPLOY_TARGET}"
log "Container: ${CONTAINER_NAME}"
log "Timestamp: $(date)"
log "Git Commit: $(git rev-parse --short HEAD 2>/dev/null || echo 'N/A')"

# Show running containers
docker ps --filter "name=${CONTAINER_NAME}"
```

---

## Switch Script

The switch script handles switching traffic between primary and secondary sites.

### `switch.sh` example

```bash
#!/bin/bash

#######################################
# Switch Script - Toggle between Primary and Secondary
# Usage: ./switch.sh
#######################################

set -e

DEPLOY_PATH="${DEPLOY_PATH:-/home/deploy/{PROJECT_NAME}}"
NGINX_CONFIG="/etc/nginx/conf.d/{project_name}.conf"
STATE_FILE="${DEPLOY_PATH}/.active_target"

# Colors
RED='\033[0;31m'
GREEN='\033[0;32m'
YELLOW='\033[1;33m'
NC='\033[0m'

echo -e "${GREEN}========================================${NC}"
echo -e "${GREEN}  Traffic Switch Script${NC}"
echo -e "${GREEN}========================================${NC}"

# Get current active target
CURRENT_TARGET="primary"
if [ -f "$STATE_FILE" ]; then
    CURRENT_TARGET=$(cat "$STATE_FILE")
fi

# Determine new target
if [ "$CURRENT_TARGET" == "primary" ]; then
    NEW_TARGET="secondary"
    NEW_PORT="3001"
else
    NEW_TARGET="primary"
    NEW_PORT="3000"
fi

echo -e "${YELLOW}Current active: ${CURRENT_TARGET}${NC}"
echo -e "${YELLOW}Switching to: ${NEW_TARGET}${NC}"

# Verify new target is healthy
HEALTH_URL="http://localhost:${NEW_PORT}/health"
if ! curl -sf "$HEALTH_URL" > /dev/null 2>&1; then
    echo -e "${RED}Error: ${NEW_TARGET} is not healthy!${NC}"
    echo -e "${RED}Cannot switch traffic to unhealthy target.${NC}"
    exit 1
fi

# Update nginx configuration (example)
if [ -f "$NGINX_CONFIG" ]; then
    echo -e "${YELLOW}Updating nginx configuration...${NC}"
    sed -i "s/proxy_pass http:\/\/localhost:[0-9]*/proxy_pass http:\/\/localhost:${NEW_PORT}/" "$NGINX_CONFIG"

    # Reload nginx
    nginx -t && nginx -s reload
fi

# Update state file
echo "$NEW_TARGET" > "$STATE_FILE"

echo -e "${GREEN}========================================${NC}"
echo -e "${GREEN}  Switch Complete!${NC}"
echo -e "${GREEN}========================================${NC}"
echo -e "Active target: ${NEW_TARGET}"
echo -e "Port: ${NEW_PORT}"
```

---

## Rollback Script

### `rollback.sh` example

```bash
#!/bin/bash

#######################################
# Rollback Script - Quick rollback to previous version
# Usage: ./rollback.sh
#######################################

set -e

DEPLOY_PATH="${DEPLOY_PATH:-/home/deploy/{PROJECT_NAME}}"
STATE_FILE="${DEPLOY_PATH}/.active_target"

# Colors
RED='\033[0;31m'
GREEN='\033[0;32m'
YELLOW='\033[1;33m'
NC='\033[0m'

echo -e "${RED}========================================${NC}"
echo -e "${RED}  ROLLBACK INITIATED${NC}"
echo -e "${RED}========================================${NC}"

# Get current active target
CURRENT_TARGET="primary"
if [ -f "$STATE_FILE" ]; then
    CURRENT_TARGET=$(cat "$STATE_FILE")
fi

echo -e "${YELLOW}Current active: ${CURRENT_TARGET}${NC}"

# Execute switch to rollback
echo -e "${YELLOW}Executing rollback (switching to previous target)...${NC}"
./switch.sh

echo -e "${GREEN}Rollback completed!${NC}"
```

---

## Docker Compose Files

These compose files define the complete application cluster for each site.

### Base `docker-compose.yml` example

```yaml
version: '3.8'

services:
  # API Application
  api:
    build:
      context: ./apps/api
      dockerfile: Dockerfile
    restart: unless-stopped
    environment:
      - NODE_ENV=${NODE_ENV:-production}
      - DATABASE_URL=${DATABASE_URL}
      - REDIS_URL=${REDIS_URL}
    volumes:
      - ./logs/api:/app/logs
    networks:
      - app_network
    depends_on:
      - redis
      - postgres

  # Web/Frontend Application
  web:
    build:
      context: ./apps/web
      dockerfile: Dockerfile
    restart: unless-stopped
    environment:
      - NODE_ENV=${NODE_ENV:-production}
      - API_URL=${API_URL}
    networks:
      - app_network

  # Background Worker Application
  worker:
    build:
      context: ./apps/worker
      dockerfile: Dockerfile
    restart: unless-stopped
    environment:
      - NODE_ENV=${NODE_ENV:-production}
      - DATABASE_URL=${DATABASE_URL}
      - REDIS_URL=${REDIS_URL}
    networks:
      - app_network
    depends_on:
      - redis
      - postgres

  # Redis Cache
  redis:
    image: redis:7-alpine
    restart: unless-stopped
    volumes:
      - redis_data:/data
    networks:
      - app_network

  # PostgreSQL Database
  postgres:
    image: postgres:15-alpine
    restart: unless-stopped
    environment:
      - POSTGRES_DB=${POSTGRES_DB}
      - POSTGRES_USER=${POSTGRES_USER}
      - POSTGRES_PASSWORD=${POSTGRES_PASSWORD}
    volumes:
      - postgres_data:/var/lib/postgresql/data
    networks:
      - app_network

  # Nginx Reverse Proxy
  nginx:
    image: nginx:alpine
    restart: unless-stopped
    ports:
      - '${NGINX_PORT:-80}:80'
      - '${NGINX_SSL_PORT:-443}:443'
    volumes:
      - ./nginx/nginx.conf:/etc/nginx/nginx.conf:ro
      - ./nginx/ssl:/etc/nginx/ssl:ro
    networks:
      - app_network
    depends_on:
      - api
      - web

volumes:
  redis_data:
  postgres_data:

networks:
  app_network:
    driver: bridge
```

### `docker-compose.primary.yml` exapmle

Site-specific overrides for the **Primary Datacenter (Site A)**:

```yaml
version: '3.8'

services:
  api:
    container_name: ${PROJECT_NAME}_api_primary
    env_file:
      - .env.primary
    healthcheck:
      test: ['CMD', 'curl', '-f', 'http://localhost:3000/health']
      interval: 30s
      timeout: 10s
      retries: 3
      start_period: 40s

  web:
    container_name: ${PROJECT_NAME}_web_primary
    env_file:
      - .env.primary

  worker:
    container_name: ${PROJECT_NAME}_worker_primary
    env_file:
      - .env.primary

  postgres:
    container_name: ${PROJECT_NAME}_postgres_primary
    # Primary database - read/write
    environment:
      - POSTGRES_REPLICATION_MODE=master

  redis:
    container_name: ${PROJECT_NAME}_redis_primary

  nginx:
    container_name: ${PROJECT_NAME}_nginx_primary
    ports:
      - '80:80'
      - '443:443'
```

### `docker-compose.secondary.yml` example

Site-specific overrides for the **Secondary Datacenter (Site B)**:

```yaml
version: '3.8'

services:
  api:
    container_name: ${PROJECT_NAME}_api_secondary
    env_file:
      - .env.secondary
    healthcheck:
      test: ['CMD', 'curl', '-f', 'http://localhost:3000/health']
      interval: 30s
      timeout: 10s
      retries: 3
      start_period: 40s

  web:
    container_name: ${PROJECT_NAME}_web_secondary
    env_file:
      - .env.secondary

  worker:
    container_name: ${PROJECT_NAME}_worker_secondary
    env_file:
      - .env.secondary

  postgres:
    container_name: ${PROJECT_NAME}_postgres_secondary
    # Secondary database - replica mode
    environment:
      - POSTGRES_REPLICATION_MODE=slave
      - POSTGRES_MASTER_HOST=${PRIMARY_DB_HOST}
      - POSTGRES_MASTER_PORT=${PRIMARY_DB_PORT}

  redis:
    container_name: ${PROJECT_NAME}_redis_secondary

  nginx:
    container_name: ${PROJECT_NAME}_nginx_secondary
    ports:
      - '80:80'
      - '443:443'
```

---

## Environment Files

### `.env.primary` example

```bash
# ===========================================
# Primary Site Configuration (Datacenter A)
# ===========================================

# Site Identification
SITE_ID=primary
SITE_NAME="Datacenter A"
INSTANCE_TYPE=primary

# Application
NODE_ENV=production
APP_PORT=3000
API_URL=https://api.example.com
LOG_LEVEL=info

# Database (Primary - Read/Write)
DATABASE_URL=postgresql://user:pass@postgres:5432/app_db
POSTGRES_DB=app_db
POSTGRES_USER=app_user
POSTGRES_PASSWORD=secure_password

# Redis
REDIS_URL=redis://redis:6379

# Nginx
NGINX_PORT=80
NGINX_SSL_PORT=443
```

### `.env.secondary` example

```bash
# ===========================================
# Secondary Site Configuration (Datacenter B)
# ===========================================

# Site Identification
SITE_ID=secondary
SITE_NAME="Datacenter B"
INSTANCE_TYPE=secondary

# Application
NODE_ENV=production
APP_PORT=3000
API_URL=https://api-secondary.example.com
LOG_LEVEL=info

# Database (Replica - Read-Only until promotion)
DATABASE_URL=postgresql://user:pass@postgres:5432/app_db
POSTGRES_DB=app_db
POSTGRES_USER=app_user
POSTGRES_PASSWORD=secure_password
PRIMARY_DB_HOST=db-primary.example.com
PRIMARY_DB_PORT=5432

# Redis
REDIS_URL=redis://redis:6379

# Nginx
NGINX_PORT=80
NGINX_SSL_PORT=443
```

---

## Quick Reference Commands

### Initial Setup (Per Site) example

```bash
# Clone the repository on each site
./clone.sh main production

# Deploy cluster to primary site
./deploy.sh primary

# Deploy cluster to secondary site (on secondary server)
./deploy.sh secondary
```

### Standard Deployment (Zero-Downtime)

```bash
# 1. Deploy new version to secondary site cluster
./deploy.sh secondary

# 2. Validate all apps are healthy on secondary
curl -f https://api-secondary.example.com/health

# 3. Switch traffic to secondary site
./switch.sh

# 4. Deploy same version to primary site (now standby)
./deploy.sh primary
```

### Emergency Rollback

```bash
# Quick rollback - switch traffic back to previous site
./rollback.sh
```

### Force Redeploy

```bash
# Force redeploy entire cluster with app removal
./deploy.sh primary --force
```

### Check Cluster Status

```bash
# View all running apps in the cluster
docker-compose -f docker-compose.yml -f docker-compose.primary.yml ps

# View logs for all apps
docker-compose -f docker-compose.yml -f docker-compose.primary.yml logs -f
```

---

## Best Practices

1. **Always deploy to Secondary site first** — Stage and test new releases on the standby cluster before switching production traffic
2. **Validate all apps** — Run health checks on every application in the cluster (API, Web, Worker, Database) before switching
3. **Keep both sites synchronized** — After switching, deploy the same version to the standby site for failover readiness
4. **Monitor cluster health** — Use Prometheus/Grafana to monitor all apps across both sites
5. **Use tagged images** — Tag Docker images with git commit SHA for version traceability across sites
6. **Database replication** — Ensure PostgreSQL replication is healthy before any switch
7. **DNS TTL** — Keep DNS TTL low (e.g., 60s) to enable fast traffic switching
8. **Document site details** — Maintain a runbook with IP addresses, credentials, and contact info for each datacenter

---

## Troubleshooting

### App not starting

```bash
# Check logs for a specific app
docker logs {project_name}_api_primary
docker logs {project_name}_worker_primary

# Check all app logs
docker-compose -f docker-compose.yml -f docker-compose.primary.yml logs

# Validate compose configuration
docker-compose -f docker-compose.yml -f docker-compose.primary.yml config
```

### Health check failing

```bash
# Test API health endpoint
curl -v http://localhost:3000/health

# Test individual apps
docker-compose -f docker-compose.yml -f docker-compose.primary.yml ps

# Check if ports are accessible
netstat -tlnp | grep -E '(80|443|3000|5432|6379)'
```

### Database replication issues

```bash
# Check PostgreSQL replication status (on primary)
docker exec {project_name}_postgres_primary psql -U app_user -c "SELECT * FROM pg_stat_replication;"

# Check replica status (on secondary)
docker exec {project_name}_postgres_secondary psql -U app_user -c "SELECT pg_is_in_recovery();"
```

### Rollback not working

```bash
# Check current active site
cat /home/deploy/{project_name}/.active_target

# Verify secondary site is healthy before rollback
curl -f https://api-secondary.example.com/health

# Manually update DNS/Load Balancer if automated switch fails
# (Refer to your DNS provider or load balancer documentation)
```

### Inter-app communication issues

```bash
# Check network connectivity between apps
docker network inspect {project_name}_app_network

# Test connectivity from API to database
docker exec {project_name}_api_primary nc -zv postgres 5432

# Test connectivity from API to Redis
docker exec {project_name}_api_primary nc -zv redis 6379
```

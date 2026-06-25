# Distributed AI Project Companion - Config Server Repository

Centralized configuration repository for the Distributed AI Project Companion microservices ecosystem.

This repository contains environment-specific configuration files consumed by the Spring Cloud Config Server and shared across all services in the platform.

---

## Overview

The Distributed AI Project Companion platform follows a microservices architecture consisting of:

- Account Service
- Workspace Service
- Intelligence Service
- API Gateway
- Eureka Service Discovery
- PostgreSQL Databases
- Redis Cache
- Kafka Messaging
- MinIO Object Storage
- AI Integration (OpenRouter/OpenAI Compatible APIs)

This repository serves as the single source of truth for application configuration across local and Kubernetes deployments.

---

## Repository Structure

```text
.
├── application.yml
├── api-gateway.yml
├── account-service.yml
├── workspace-service.yml
├── intelligence-service.yml
└── README.md
```

---

## Services

### Account Service

Configuration file:

```text
account-service.yml
```

Responsibilities:

- User authentication
- User registration
- Account management
- Stripe payment integration
- Webhook processing

Default Port:

```text
9050
```

Context Path:

```text
/account
```

Dependencies:

- PostgreSQL
- Stripe API

---

### Workspace Service

Configuration file:

```text
workspace-service.yml
```

Responsibilities:

- Project management
- Workspace management
- File storage
- Redis caching
- MinIO integration

Default Port:

```text
9020
```

Context Path:

```text
/workspace
```

Dependencies:

- PostgreSQL
- Redis
- MinIO

---

### Intelligence Service

Configuration file:

```text
intelligence-service.yml
```

Responsibilities:

- AI-powered code generation
- AI chat interactions
- Embedding generation
- Intelligent project assistance

Default Port:

```text
9030
```

Context Path:

```text
/intelligence
```

Dependencies:

- PostgreSQL
- OpenRouter/OpenAI-compatible APIs

Configured Models:

| Feature | Model |
|----------|---------|
| Chat | google/gemini-3-flash-preview |
| Embeddings | qwen/qwen3-embedding-8b |

---

### API Gateway

Configuration file:

```text
api-gateway.yml
```

Default Port:

```text
8080
```

Responsibilities:

- Request routing
- API aggregation
- Security enforcement
- Public route management

Routes:

| Route | Destination |
|---------|-------------|
| /api/v1/account/** | Account Service |
| /api/v1/workspace/** | Workspace Service |
| /api/v1/intelligence/** | Intelligence Service |

Public Endpoints:

```text
/api/v1/account/auth/login
/api/v1/account/auth/signup
/api/v1/account/webhooks/**
/api/v1/workspace/public/**
```

---

## Shared Configuration

Global configuration is defined in:

```text
application.yml
```

Includes:

- JPA configuration
- Kafka configuration
- Eureka discovery settings
- JWT configuration
- Frontend integration settings
- Management endpoints
- Logging configuration

---

## Local Development Environment

### Infrastructure Requirements

| Component | Default Port |
|------------|-------------|
| PostgreSQL | 9010 |
| Redis | 6379 |
| Kafka | 29092 |
| MinIO | 9000 |
| Eureka | 8761 |
| API Gateway | 8080 |

### Local Databases

Account Service:

```text
local_account_db
```

Workspace Service:

```text
local_workspace_db
```

Intelligence Service:

```text
local_intelligence_db
```

---

## Kubernetes Deployment

The repository supports Kubernetes deployment using Spring Profiles.

Activate:

```bash
-Dspring.profiles.active=k8s
```

Kubernetes-specific configurations include:

- Internal service discovery
- Cluster-local PostgreSQL
- Cluster-local Redis
- Cluster-local Kafka
- Internal MinIO access
- Environment-variable-based secrets

---

## Required Environment Variables

### Common

```bash
JWT_SECRET=
DB_PASSWORD=
```

### Intelligence Service

```bash
AI_API_KEY=
```

### Stripe Integration

```bash
STRIPE_API_KEY=
STRIPE_WEBHOOK_SECRET=
```

### MinIO

```bash
MINIO_ROOT_USER=
MINIO_ROOT_PASSWORD=
```

### Frontend & Preview

```bash
FRONTEND_URL=
PREVIEW_DOMAIN=
PREVIEW_NAMESPACE=
PROXY_PORT=
```

---

## Service Discovery

Local environments use Eureka:

```yaml
eureka:
  client:
    enabled: true
```

Kubernetes environments disable Eureka and rely on Kubernetes DNS:

```yaml
eureka:
  client:
    enabled: false
```

---

## Messaging

Kafka is used for asynchronous communication between services.

Local:

```text
localhost:29092
```

Kubernetes:

```text
kafka.distributed-ai-core.svc.cluster.local:9092
```

---

## Storage

### PostgreSQL

Primary persistence layer for:

- Accounts
- Workspaces
- AI metadata

### Redis

Used for:

- Caching
- Session management
- Temporary workspace state

### MinIO

Used for:

- Project files
- Generated artifacts
- Workspace assets

Bucket:

```text
projects
```

---

## Security

The platform uses:

- JWT Authentication
- Gateway-level route protection
- Environment-based secret management
- Stripe webhook validation

**Important:** Never commit production secrets to this repository.

---

## Getting Started

1. Start infrastructure services:
   - PostgreSQL
   - Redis
   - Kafka
   - MinIO
   - Eureka Server

2. Start Spring Cloud Config Server.

3. Launch services:
   - Account Service
   - Workspace Service
   - Intelligence Service
   - API Gateway

4. Verify service registration in Eureka.

5. Access APIs through the Gateway:

```text
http://localhost:8080
```

---

## Architecture

```text
                    +----------------+
                    | Config Server  |
                    +-------+--------+
                            |
        +-------------------+-------------------+
        |                   |                   |
        v                   v                   v

+---------------+  +---------------+  +---------------+
| Account       |  | Workspace     |  | Intelligence  |
| Service       |  | Service       |  | Service       |
+-------+-------+  +-------+-------+  +-------+-------+
        \                 |                 /
         \                |                /
          \               |               /
           +--------------+--------------+
                          |
                          v
                  +---------------+
                  | API Gateway   |
                  +---------------+

Supporting Services:
- PostgreSQL
- Redis
- Kafka
- MinIO
- Eureka
```

---

## License

This project is part of the Distributed AI Project Companion ecosystem.

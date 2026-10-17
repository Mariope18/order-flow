# OrderFlow — Distributed Event-Driven Microservices

Enterprise-grade portfolio backend showcasing distributed microservices architecture, Hexagonal Architecture (Ports & Adapters), Transactional Outbox Pattern over Apache Kafka, and end-to-end distributed observability.

---

## 🏗️ Architecture & Tech Stack

- **Language & Runtime:** Java 21 LTS
- **Framework:** Spring Boot 4.1.x
- **Architecture Pattern:** Hexagonal Architecture (Domain Core, Ports & Inbound/Outbound Adapters)
- **Database:** PostgreSQL 16 (Database-per-Service pattern)
- **Message Broker:** Apache Kafka 3.8 (KRaft mode, no ZooKeeper)
- **Observability:** OpenTelemetry & Jaeger (Distributed Tracing), Prometheus
- **Resilience:** Virtual Threads (Project Loom), Resilience4j

---

## 📦 Microservices

| Service | Responsibility | Port | Database |
|---|---|---|---|
| `order-service` | Order placement, lifecycle & saga coordinator | `8081` | `order_db` |
| `inventory-service` | Real-time stock reservation & optimistic locking | `8082` | `inventory_db` |
| `payment-service` | Asynchronous payment processing & idempotency | `8083` | `payment_db` |
| `orderflow-common` | Shared domain primitives, events & error models | N/A (Library) | N/A |

---

## 🐳 Local Infrastructure (Docker Compose)

The repository provides a single `docker-compose.yml` to spin up all required support infrastructure.

### Services & Ports

| Component | Container Name | Host Port | Internal Port | Description / UI |
|---|---|---|---|---|
| **PostgreSQL 16** | `orderflow-postgres` | `5432` | `5432` | Single instance auto-provisioning `order_db`, `inventory_db`, `payment_db` |
| **Apache Kafka** | `orderflow-kafka` | `9092` | `29092` / `9092` | KRaft mode event broker |
| **Jaeger UI** | `orderflow-jaeger` | `16686` | `16686` | Distributed Tracing UI ([http://localhost:16686](http://localhost:16686)) |
| **OTLP Collector** | `orderflow-jaeger` | `4317` / `4318` | `4317` / `4318` | OpenTelemetry gRPC / HTTP collectors |

### Quick Start

```bash
# Start all infrastructure in background
docker compose up -d

# Verify containers health status
docker compose ps

# View logs for a specific service
docker compose logs -f kafka
docker compose logs -f postgres

# Stop all containers (preserving database and kafka data)
docker compose down

# Stop and wipe persistent volumes (clean slate)
docker compose down -v
```

### PostgreSQL Credentials

- **Host:** `localhost`
- **Port:** `5432`
- **Username:** `orderflow`
- **Password:** `orderflow_password`
- **Provisioned Databases:**
  - `order_db`
  - `inventory_db`
  - `payment_db`
  - `postgres` (admin default)

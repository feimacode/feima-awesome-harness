---
preset: java-microservices
label: "Java Microservices"
description: "Spring Boot microservices in a Maven monorepo. Production-grade Java setup with service discovery, containerisation, and GitHub Actions CI."
---

# Preset: Java Microservices

## Pre-filled answers

### Repo Structure
- **Topology:** MonoRepo
- **Monorepo tooling:** Maven (multi-module, no extra orchestrator)

### Tech Stack
- **Language / Runtime:** Java / JVM (Java 21 LTS)
- **Build tooling:** Maven
- **Database:** Relational / RDBMS — PostgreSQL
- **API style:** REST / HTTP + gRPC / Protobuf (inter-service)
- **Delivery target:** Library / SDK (containerised services — no browser front-end in this repo)
- **Unit testing:** JUnit 5 + Mockito
- **E2E / integration testing:** Testcontainers (per-service integration tests against real Postgres)
- **Linting & formatting:** Checkstyle + google-java-format
- **Cloud:** Partially — some managed services

### AI Tools
- **AI coding assistant:** GitHub Copilot (VS Code agent mode)
- **AI methodology:** No — ad-hoc
- **Shell preference:** bash
- **Tool use style:** No preference
- **Confirmation:** Ask before destructive actions

### CI/CD
- **Platform:** GitHub Actions
- **IaC:** Docker Compose only (local dev); Helm (Kubernetes) for production

## Typical project layout

```
services/
  order-service/
    src/main/java/…
    src/test/java/…
    pom.xml
  user-service/
    src/main/java/…
    src/test/java/…
    pom.xml
  gateway/           # Spring Cloud Gateway
    pom.xml
shared/
  api-contracts/     # Proto definitions or shared DTOs
  common/            # shared utils, error types
pom.xml              # parent POM
docker-compose.yml   # local stack (services + Postgres + Kafka if needed)
```

## Why these choices?

| Choice | Rationale |
|--------|-----------|
| Spring Boot 3 | Industry standard; production-ready auto-configuration |
| Java 21 | LTS with virtual threads (Project Loom) for high throughput |
| Maven multi-module | Native multi-module support; reproducible builds |
| Spring Cloud Gateway | Routing, load-balancing, auth filter in one place |
| gRPC inter-service | Strongly typed contracts; lower overhead than REST internally |
| Testcontainers | Real Postgres in CI without a separate service; no mocking DB |
| Checkstyle + google-java-format | Consistent code style across many contributors |
| Helm | De-facto Kubernetes package manager for production deploy |
| GitHub Actions | Matrix builds across Java versions; easy Docker image push |

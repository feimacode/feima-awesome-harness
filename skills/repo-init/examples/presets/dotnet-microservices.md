---
preset: dotnet-microservices
label: ".NET Microservices"
description: "ASP.NET Core microservices in a solution monorepo. Modern .NET 9 setup with minimal APIs, MassTransit messaging, Testcontainers, and GitHub Actions CI."
---

# Preset: .NET Microservices

## Pre-filled answers

### Repo Structure
- **Topology:** MonoRepo
- **Monorepo tooling:** MSBuild / dotnet CLI (solution file — no extra orchestrator)

### Tech Stack
- **Language / Runtime:** .NET (C#)
- **Build tooling:** MSBuild / dotnet CLI
- **Database:** Relational / RDBMS — PostgreSQL (via Npgsql + EF Core)
- **API style:** REST / HTTP + gRPC / Protobuf (inter-service)
- **Delivery target:** Library / SDK (containerised services — no browser front-end in this repo)
- **Unit testing:** xUnit + NSubstitute
- **E2E / integration testing:** Testcontainers.DotNet (per-service integration tests)
- **Linting & formatting:** Roslyn analyzers + dotnet-format
- **Cloud:** Partially — some managed services

### AI Tools
- **AI coding assistant:** GitHub Copilot (VS Code agent mode)
- **AI methodology:** No — ad-hoc
- **Shell preference:** No preference
- **Tool use style:** No preference
- **Confirmation:** Ask before destructive actions

### CI/CD
- **Platform:** GitHub Actions
- **IaC:** Docker Compose only (local dev); Azure Bicep (production)

## Typical project layout

```
src/
  OrderService/
    OrderService.Api/          # ASP.NET Core minimal API
    OrderService.Core/         # domain + application logic
    OrderService.Infrastructure/
  UserService/
    UserService.Api/
    UserService.Core/
    UserService.Infrastructure/
  SharedKernel/                # shared value objects, base classes
tests/
  OrderService.UnitTests/
  OrderService.IntegrationTests/
  UserService.UnitTests/
docker-compose.yml
Feima.sln                      # solution file wires all projects
```

## Why these choices?

| Choice | Rationale |
|--------|-----------|
| .NET 9 minimal APIs | Lower ceremony than controllers; excellent performance |
| EF Core + Npgsql | First-class ORM for Postgres with good migration tooling |
| MassTransit | Abstracts RabbitMQ/Azure Service Bus; saga support built-in |
| gRPC inter-service | Strongly typed contracts; .NET protobuf tooling is mature |
| xUnit | Recommended by the .NET team; clean parallel test execution |
| NSubstitute | Readable mocking API; better than Moq for modern C# |
| Testcontainers.DotNet | Real Postgres in CI without infra setup |
| Roslyn analyzers | Catches issues at compile time; enforces code style |
| Azure Bicep | Native ARM templating; cleaner than raw JSON ARM |
| GitHub Actions | Matrix builds across .NET versions; easy Docker push |

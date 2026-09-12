# Contracts and data

## Authoritative sources

| Boundary | Source | Outputs |
| --- | --- | --- |
| Browser → BFF | BFF OpenAPI | TypeScript client; runtime schemas where useful |
| BFF → service | Provider OpenAPI | Typed clients/models in consumer languages |
| Events | AsyncAPI + JSON Schema payloads | Producer/consumer models, docs, compatibility checks |
| Configuration | Language-native typed schema | Safe environment example and generated reference |

Provider OpenAPI is generated from route schemas; authoring it by hand instead needs a project ADR. Each boundary has one authority, deterministic generation, and a versioned compatibility baseline. Generated files carry do-not-edit headers and are never manually maintained. CI validates schemas and fails when regeneration changes committed output. Compile generated consumers where practical.

Separate HTTP, event, domain, and persistence models through explicit mappings. Prefer additive evolution; deprecate with an end date before removal. Breaking changes require a project ADR, migration strategy, and explicit consumer plan. A clean drift check is not proof of compatibility with deployed consumers.

## Communication

| Need | Mechanism |
| --- | --- |
| Immediate answer/command result | HTTP or gRPC, bounded timeout and caller-visible failure |
| Durable fact occurred | Kafka event; consumer-owned processing/retry state |
| Resilient cross-domain read composition | Locally owned materialized projection |
| Strong transaction spanning services | Revisit the capability boundary first |
| Long-running process | Explicit workflow state, idempotency, timeouts and compensation |

## Events

Use capability-owned past-tense names, e.g. `data-product.created.v1` and `data-product.published.v1`. Every envelope includes event ID, type, version, occurred-at timestamp, producer, trace context, and payload. Select exact field names/encoding in the contract, not independently in each language.

Where persistence/publication consistency matters, use an outbox or equivalent atomic publication. Consumers persist deduplication or natural idempotency state when necessary. Bound exponential-backoff retries and define a poison-message recovery path. Check schema compatibility before merge and deployment. Partition only for domain-required ordering. Document replay and backfill before the first production consumer, including validation and recovery using the [runbook template](knowledge-base.md#runbook-template).

## Persistence

A service never queries another service's tables. Integrate through APIs, events, or a locally owned projection. Each deployable owns its database/schema, credentials, and migrations. A modular monolith keeps capability ownership explicit before extraction.

Migrations remain compatible with the currently deployed application during rolling deployment; use expand/migrate/contract for destructive evolution. Run migrations through a controlled release job, not competing replicas. Record retention, classification, tenancy, backup/restore, and deletion expectations in service metadata. Read replicas and analytical sinks consume owned data; they do not grant cross-service write access.

See [testing and delivery](delivery.md) for contract, integration, migration, and generated-output validation.

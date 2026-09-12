# Operations and security

## Mandatory capabilities

| Capability | Minimum implementation |
| --- | --- |
| Health | Distinct `GET /health/live` and `GET /health/ready` |
| Configuration | Typed, environment-driven, validated at startup, documented; fail fast |
| Logging | Structured JSON deployed; optional readable local format |
| Tracing | OpenTelemetry HTTP/client/database instrumentation and context propagation |
| Metrics | Request count/duration/errors plus domain signals |
| Errors | Stable categories, no client-visible secrets or stack traces |
| Shutdown | Stop accepting work, drain requests, close consumers/connections |
| Security | Non-root image, dependency checks, least privilege |
| Ownership | CODEOWNERS/service metadata: contact, tier, dependencies and data owner |
| Documentation | Purpose, local run, APIs/events, config, operations and failure modes |

Readiness checks dependencies required to serve traffic. Liveness detects a process that cannot recover without restart. Consumers and workers expose the same two endpoints over a minimal HTTP listener: readiness covers broker connection and consumer-group assignment.

## Local development

Run applications natively with watch mode, stateful infrastructure in Compose. Provide PostgreSQL, a Kafka-compatible broker, Redis, and telemetry as needed by the selected slice; record infrastructure additions and avoid starting unused dependencies. Pin runtimes in a checked-in tool-version mechanism consistent with workspace asdf. Document clean-machine prerequisites, install/bootstrap, seed, first journey, and cleanup without hidden steps.

Reserve deterministic port ranges with one generated reference page. Seed/reset scripts target only project-owned local volumes; ordinary `infra:down` preserves data. Support selected-process startup through Turbo filters (`pnpm dev --filter @apps/web...`), verifying the syntax against the pinned Turbo version. Replace external providers with local fakes or explicit sandboxes. Document [root commands](development.md#universal-tasks) in the product repository only after they exist.

## Identity and security

The BFF terminates browser session concerns; services verify identity/claims with least privilege. Owning capabilities enforce authorization; BFF checks supplement them. Select the authentication model explicitly and document it before production use.

Use a managed deployed secret store. Safe `.env.example` values include descriptions; ignore real `.env` files. Never commit, log, or bake secrets into images. Commit native lockfiles; automate vulnerability and licence checks with a deliberate update cadence and exception process. Generate SBOM/provenance as production maturity requires.

## Correlation and signals

Propagate trace context (`traceparent`) and `x-request-id` across browser → BFF → services → Kafka → consumers using transport-appropriate headers. Log trace_id, span_id, request_id, service, environment, version, event type, and stable error code where applicable. Validate the trust boundary for incoming correlation and identity headers in the implementation.

Exclude raw credentials, session tokens, sensitive payloads, and unrestricted personal data from telemetry. Define SLIs before alerts; alert on user impact. Every production service has a dashboard and linked operational runbook. Demonstrate correlation with an actual request, not merely instrumentation configuration.

## Containers and deployment

Use multi-stage builds, pinned inputs, minimal practical runtime images, a non-root user, and a read-only filesystem where possible. One process responsibility per container; expose the HTTP port. Inject configuration into environment-agnostic images.

After the local slice, contracts and CI are reliable, supply tier-appropriate Kubernetes liveness/readiness/startup probes, resource requests/limits, graceful termination, and PodDisruptionBudget. Include service/version/environment/ownership labels and telemetry resource attributes. Control migrations through a pre-deploy/release job. Demonstrate progressive rollout, health/SLO gates, rollback, post-deploy smoke and deployment annotations in a disposable environment.

A production runbook covers purpose, signals/SLOs, upstream/downstream/data/topic dependencies, symptoms and confirmation queries, customer impact, reversible mitigation, recovery/reconciliation/communication, and owner/backup escalation thresholds. Use the [runbook template](knowledge-base.md#runbook-template).

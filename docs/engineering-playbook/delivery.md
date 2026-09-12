# Delivery and implementation

## Operating protocol

Inspect the target repository, instructions, status, and adopted decisions; preserve unrelated work. Track execution in workspace beads. Map the authorized scope to WP IDs below and present a short file-level plan. Implement the smallest complete slice, keeping the repository runnable after each phase. Validate meaningful batches and repair failures before expansion. Change examples and docs together; record material deviations as project ADRs.

The first feature lists data products and creates a draft with validated name and owner, and persists it in PostgreSQL. Browser → generated BFF client → Fastify BFF → TypeScript data-products capability → owned database. Domain rules and persistence stay in the capability. WP-04 defines the `data-product.created.v1` contract and writes its outbox row; publishing to Kafka lands in WP-06, so the first slice does not require a broker.

## Testing

| Layer | Purpose | Default |
| --- | --- | --- |
| Unit | Domain rules/pure transformations | Vitest, go test, pytest |
| Component | React behavior/accessibility | Testing Library + Vitest |
| Service integration | Adapters with real infrastructure | Testcontainers or Compose profile |
| Contract | Provider/consumer compatibility | Schema validation + generated-client compilation |
| E2E | Critical SPA/BFF journeys | Playwright |
| Migration | Forward migration and rolling compatibility | Ephemeral CI database |
| Smoke | Deployed health + critical path | Lightweight post-deploy job |

The pyramid is a cost heuristic, not a quota. Give tests ownership, deterministic setup, useful failure output, and the lowest useful layer. In-memory substitutes do not demonstrate compatibility with the production database engine.

## CI/CD

1. Validate lockfiles, workspace graph, formatting, lint, types, schemas, generated drift.
2. Run unit/component and changed-scope integration/migration tests.
3. Build affected graph, immutable artifacts and deployable images.
4. Scan dependencies/images, secrets and licences; add SBOM/provenance as maturity grows.
5. Provide previews where valuable, contract checks and E2E smoke.
6. Release commit/version-labelled images, notes, migration plan and approval policy.
7. Deploy progressively with health/SLO gates and automatic or documented rollback.
8. Verify smoke, telemetry and deployment annotations.

Turbo determines affected work, but complete native/contract dependency edges and cache inputs are prerequisites. See [task contracts](development.md#universal-tasks). Full checks must remain available when affected execution cannot prove coverage.

## Generators

`create-project <name>` creates React/BFF, pnpm/Turbo, shared TypeScript/lint/test config, example capability, contracts/generation, Compose, OTel, CI, docs and initial ADRs. Its root entrypoint is `pnpm create-project <name>`; `create:app` creates a React/BFF pair, `create:service` a capability, and `create:package` a shared package. WP-10 defines two things this roadmap leaves open: how `create-project` runs before a product repository exists, and the `create:package` input contract.

| Service input | Choices/default |
| --- | --- |
| Name | Required kebab-case capability |
| Language | TypeScript, Go, Python |
| Shape | HTTP, event consumer, worker, combined |
| Persistence | None, PostgreSQL |
| Events | Produces, consumes, neither |
| Tier | Experimental, internal, production-critical |
| Owner | Required team identifier |

Only the TypeScript framework default is fixed to Fastify; NestJS needs explicit selection and rationale. Do not invent silent defaults for unselected language/shape/persistence.

Generators support non-interactive flags, fail safely on an existing target, are idempotent where feasible, contain no secrets/organization-only values, format output, and print exact next commands/verification. Document upgrades; do not silently rewrite mature services.

Generated-service acceptance: automatic workspace/task membership; all applicable tasks work with documented justified no-ops; health probes; structured logs and a request/message trace; passing unit/integration examples; non-root container and graceful shutdown; registered ownership/ports/dependencies/contracts/docs; golden tests for unintended scaffold drift. Also run generated output's own checks: golden equality alone cannot prove it works.

## Work packages

This is the roadmap, not a duplicate issue-status tracker. Create/claim concrete beads issues when implementation is authorized; reference these IDs in them. Product implementation is not started by integrating this document.

| ID | Package | Scope and exit |
| --- | --- | --- |
| WP-01 | Foundation | Layout, root scripts, Turbo, pinned versions, lint/format, README; clean install/check |
| WP-02 | React | Vite/router/query/forms/validation/accessibility; running app, component test, client boundary |
| WP-03 | BFF | Fastify/config/auth seam/OpenAPI/errors/context/health/downstream client; SPA reaches BFF, schema validates |
| WP-04 | Vertical slice | Capability/domain rule/PostgreSQL/migration/repository/tests; create/list through browser and BFF |
| WP-05 | Polyglot | Go/Python examples and native adapters; root build/lint/typecheck/test across languages |
| WP-06 | Contracts | HTTP/events, generation, drift/compatibility; CI rejects invalid schemas or changed output |
| WP-07 | Local platform | Compose, ports, seed/reset, filtered startup; clean-machine tutorial works |
| WP-08 | Observability | Collector/backend, propagation/logs/metrics; correlate BFF/service request |
| WP-09 | CI/images | Affected checks, integration/E2E, scanning/artifacts/images; PR and main image pipelines green |
| WP-10 | Generators | Project/app/service/package generators, golden tests; fresh output passes checks |
| WP-11 | Knowledge base | Navigation/tutorials/standards/ADRs/generated references/link checks; unaided first change |
| WP-12 | Deployment | Kubernetes baseline/migrations/rollout/rollback/smoke; disposable deployment and rollback |

Recommended releases: WP-01–04 thin slice; WP-05–08 polyglot/operations; WP-09–11 repeatability/docs; WP-12 deployment maturity. Each phase includes its own docs and validation. Pull forward the minimal contracts, Compose, telemetry and container work needed for thin-slice acceptance; later packages generalize it rather than deferring first-slice requirements.

## Source backlog identifiers

| Priority | IDs and deliverables |
| --- | --- |
| P0 | P0-01 foundation/bootstrap; P0-02 web; P0-03 BFF; P0-04 TypeScript data-products/PostgreSQL; P0-05 connected create/list; P0-06 clean-clone quality gate |
| P1 | P1-01 HTTP clients; P1-02 event envelope/outbox/idempotency; P1-03 Go; P1-04 Python; P1-05 full local telemetry/query examples; P1-06 CI/affected checks/integration/E2E/images |
| P2 | P2-01 generators; P2-02 docs navigation/tutorial/ADR/link checking; P2-03 security scans/exceptions; P2-04 disposable deployment/migrations/rollout/smoke/rollback |

## Evidence and completion

For each implemented item: code/tests complete, no hidden manual steps, relevant root commands pass and are reported, docs current (or an explicit no-impact rationale), no secrets/machine-specific paths/unowned TODOs, operational failures considered, deviations in ADRs or tracked decisions. Tie known gaps to beads IDs.

Handoff includes exact commands and results, an example curl or browser journey, health/log/metric/trace evidence for a request, generated drift and contract results, container build and graceful shutdown results, changed files, deviations, remaining risks, and next WP. Mark unrun checks as unverified; a written command is not evidence.

Final platform acceptance requires all of:

- Clean-environment bootstrap from documented prerequisites.
- ARC-01–08 boundaries demonstrated or deviations recorded.
- Idiomatic TypeScript/Go/Python participation in root tasks.
- Schema/generation/drift/compatibility checks in CI.
- Native watch loop with containerized dependencies.
- Working health/shutdown/logs/metrics/traces.
- Green unit/integration/contract/migration/E2E/smoke examples.
- Generators and golden tests producing compliant output.
- Navigable, checked, owned/reviewed docs and successful first-change tutorial.
- Immutable images, documented rollout/rollback and post-deploy smoke.

The platform is complete only when template, generators, CI and knowledge base agree.

## Kickoff prompt

> Use platform-slice in the selected product repository for WP-01 through WP-04. Follow this playbook's approved React/Vite → Fastify BFF → TypeScript data-products → PostgreSQL design. Inspect instructions and report conflicts before edits, then give a short file-level plan by WP. Preserve unrelated work. Use native package ownership and the universal task interface. Include reproducible OpenAPI/client generation, health, typed config, structured logs, telemetry seams, shutdown, tests and docs. Demonstrate clean bootstrap, pnpm check, the create/list browser journey, valid OpenAPI and no generation drift, health, one correlated request trace, logs and container builds. Record deviations and known gaps with evidence and beads IDs; recommend the next WP.

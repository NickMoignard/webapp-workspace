# Development standards

## Defaults

| Concern | Default |
| --- | --- |
| Browser application | Standalone React + Vite + React Router; client rendering |
| Server state | TanStack Query |
| Forms/runtime validation | React Hook Form + Zod |
| BFF | TypeScript + Fastify + Zod + OpenAPI |
| TypeScript capability | Fastify; NestJS only explicitly selected with rationale |
| Go capability | net/http-compatible router and standard Go tooling |
| Python capability | uv + FastAPI + Pydantic + pytest + Ruff + mypy (strict) |
| Orchestration | pnpm workspaces + Turborepo |
| Local stateful dependencies | Docker Compose; applications run natively |
| Contracts | OpenAPI + AsyncAPI/JSON Schema |
| Telemetry | OpenTelemetry |

Pin compatible versions during implementation using verified official documentation. The playbook selects technologies, not floating latest versions. React performance guidance applies where relevant; Next.js rendering/deployment assumptions do not become Vite requirements. Jest/NestJS skills apply to projects using those tools; the default TypeScript tests here use Vitest.

## Repository contract

```text
product/
  apps/web/                 # React SPA
  apps/web-bff/             # frontend-owned BFF
  services/                # capability deployables; initially data-products
  packages/                # api-client, ui, config-eslint, config-typescript, test-utils
  contracts/               # openapi, asyncapi, json-schema
  tooling/                 # generators, scripts
  infra/                   # compose; kubernetes and terraform when justified
  docs/                    # product knowledge base and ADRs
  pnpm-workspace.yaml
  package.json
  turbo.json
  CONTRIBUTING.md
  README.md
```

Workspace globs: `apps/*`, `services/*`, `packages/*`, `tooling/*`. Go/Python services have minimal private `package.json` task adapters with identity metadata and scripts, no Node dependency ownership. `go.mod`/`go.sum` and `pyproject.toml`/`uv.lock` remain authoritative. Every new deployable under `services/` needs a capability rationale and an ADR (ARC-03). `services/<name>` is a deployable; root `contracts/` holds shared wire artifacts — a service named `contracts` is not that directory.

pnpm owns Node dependencies/linking; Turbo owns the task graph, filters, ordering, and cache; uv owns Python constraints, environments and dependencies; Go modules own Go dependencies. Compose supplies stateful dependencies and composed integration profiles, not default source compilation.

## Universal tasks

| Task | Contract | Cache |
| --- | --- | --- |
| `dev` | Persistent watch process | Off |
| `build` | Deployable output or native compilation verification; declare outputs | On when reproducible |
| `lint` | Non-mutating checks | On |
| `format` | Apply formatting | Off |
| `format:check` | Verify formatting without edits | On |
| `typecheck` | Supported static analysis | On |
| `test` | Deterministic unit/component tests | Only hermetic environments |
| `test:integration` | Tests with real infrastructure | Normally off |
| `test:e2e` | Composed critical user journeys | Off |
| `contract:check` | Schema, generated-output drift, compatibility | On with complete inputs |
| `docker:build` | Production image build | Container/CI cache as appropriate |

Document justified no-ops rather than silently omitting tasks; a no-op cannot satisfy a capability that genuinely applies. Cache inputs include source, config, native lockfiles, schemas, generators, and relevant environment declarations. Keep secrets and host-specific data out of outputs. Independently verify Turbo dependency edges for native adapters and contracts; workspace membership alone is not sufficient evidence of ordering.

Intended root commands (not implemented in this workspace):

```bash
pnpm install
pnpm bootstrap          # uv sync, go mod download, generated contracts
pnpm infra:up
pnpm dev
pnpm check              # lint, format:check, typecheck, test, contract:check
pnpm test:integration
pnpm test:e2e
pnpm create:service
pnpm create:app
pnpm infra:down          # preserve local data
```

## Frontend

Use `src/app` for router/providers/composition; `src/features/<capability>/{api,components,routes,schemas,tests}` for feature behavior; `src/shared` for genuinely shared UI/utilities; `src/generated` for the generated BFF client. The generated BFF client lives in `src/generated`. Move it to `packages/api-client` only when a second consumer exists, and then it is the only generated tree.

| State | Home |
| --- | --- |
| Server data | TanStack Query; do not mirror into context or Zustand |
| Shareable navigation | React Router URL/search parameters |
| Forms | React Hook Form; Zod at submission/external boundaries |
| Local UI | Nearby useState/useReducer |
| Cross-feature UI | Zustand only when URL/server/lifted-local state is a poor fit |

Route modules define loader/search-param boundaries; features own interactions. Components call feature query functions using the generated client, not direct fetch. Validate untrusted URLs, storage, feature flags, and API responses where required. Use semantic controls, keyboard access, visible focus, labels, and automated accessibility checks.

Provide application and route error boundaries; branch on stable BFF error codes, not messages. Use structural skeletons and bounded-action spinners. Bound retries and exclude validation, authorization, and deterministic client failures. Define optimistic updates, invalidation, and rollback per mutation.

## BFF layout

Use `src/app.ts`, `server.ts`, `config/`, `plugins/` (auth/telemetry/context), `clients/`, and `modules/<capability>/{routes,schemas,handler,mapper}.ts`, plus tests. Apply the [BFF boundary](architecture.md#bff-boundary).

## Language-specific service layouts

- TypeScript: `src/{api,application,domain,infrastructure}`, `src/server.ts`, `migrations/`, `test/`, `package.json`, `tsconfig.json`, `Dockerfile`.
- Go: `cmd/server/main.go`, `internal/{api,application,domain,repository}`, `migrations/`, `go.mod`, `go.sum`, adapter `package.json`, `Dockerfile`. Use context propagation, explicit construction, table-driven tests, gofmt, go vet, and pinned lint tools. Keep dependency construction idiomatic.
- Python: `src/<package>/{api,application,domain,infrastructure}`, `tests/`, `migrations/`, `pyproject.toml`, `uv.lock`, adapter `package.json`, `Dockerfile`. Use explicit injectable startup, FastAPI/Pydantic boundaries, Ruff, pytest, and a documented type checker.

All deployables implement the [mandatory operational capabilities](operations.md#mandatory-capabilities) and [owned persistence rules](contracts-and-data.md#persistence).

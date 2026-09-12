# Context Map

This is a **multi-context** agent workspace: it groups several projects, and each
project is its own bounded context with its own domain language and decisions.
This file is the index of those contexts. **Agents must keep it current** — when
a project is added (`make add-project`) or removed, register or remove its entry
here and update the relationships. Treat it as maintained, not optional.

See `CONTEXT.md` for the workspace's own vocabulary and `docs/agents/domain.md`
for how skills consume these files. Per-context ADRs live in
`projects/<name>/docs/adr/`; workspace-wide decisions stay in the root
`docs/adr/`. Every project context below follows the
[engineering playbook](./docs/engineering-playbook/index.md); a project's ADRs
are where its divergences from it are recorded.

## Contexts

- [Workspace tooling](./CONTEXT.md) — the workspace itself: how projects are
  referenced (manifest + symlinks), skills, toolchains, issue tracking.
- [Data Model as Code](./projects/data-model-as-code/CONTEXT.md) — four authoring layers: DBML data models, ODCS contracts, ODPS products and Ossie semantic models; DBML/Ossie integration is planned.
- [Playbook Demo](./projects/playbook-demo/CONTEXT.md) — a purpose-built reference implementation of the engineering playbook: work packages, evidence and divergence, and the target SPA → BFF → data-products capability → owned PostgreSQL system.

## Relationships

- The workspace references Data Model as Code and Playbook Demo, and provides
  their shared tooling, issue tracker and engineering playbook. Project planning
  and research documents live in each project's repository. No cross-project
  domain relationships are established.
- **Playbook Demo is the playbook's executable counterpart.** It exists to
  demonstrate the workspace's own standard (see its
  [ADR 0001](./projects/playbook-demo/docs/adr/0001-demonstrate-the-engineering-playbook.md)),
  so the dependency runs both ways: a playbook amendment that invalidates
  something demonstrated there obliges a matching change in the project, and a
  rule that proves unclear while building there is fixed in the playbook page
  rather than settled locally. Its product domain — Demo Tasks, a B2B to-do
  product sold per seat — is deliberately unrelated to data modelling, and the
  two projects share no domain vocabulary.
- **Playbook standing.** Playbook Demo is at WP-01 (pnpm/Turbo foundation,
  pinned toolchain, shared lint/format/TypeScript config; `pnpm check` green
  from a clean clone, no application yet), converging by construction rather
  than by migration; WP-02 is the next authorized step. Data Model as Code is a browser-only React + Vite SPA
  with no BFF or capability service, so most of the playbook's server-side
  surface does not yet apply to it. Its current divergences from the development
  defaults — npm rather than pnpm/Turbo, `node:test` via tsx rather than Vitest,
  no linter, and Ajv rather than Zod at boundaries — are not yet recorded as
  ADRs in the project. Record or resolve them there; `platform-review` against
  the project establishes the current position.

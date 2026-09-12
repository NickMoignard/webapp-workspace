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

## Relationships

- The workspace references Data Model as Code and provides its shared tooling,
  issue tracker and engineering playbook. Project planning and research
  documents live in the project repository. No cross-project domain
  relationships are established.
- **Playbook standing.** Data Model as Code is a browser-only React + Vite SPA
  with no BFF or capability service, so most of the playbook's server-side
  surface does not yet apply to it. Its current divergences from the development
  defaults — npm rather than pnpm/Turbo, `node:test` via tsx rather than Vitest,
  no linter, and Ajv rather than Zod at boundaries — are not yet recorded as
  ADRs in the project. Record or resolve them there; `platform-review` against
  the project establishes the current position.

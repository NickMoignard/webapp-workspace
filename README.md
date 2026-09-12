# webapp-workspace

Engineering workspace for standalone React applications, TypeScript BFFs,
and capability-aligned TypeScript, Go, and Python services.

This is an **agent workspace**: a parent directory that references several
projects (as symlinks to shared clones) so an agent and a human in VS Code can
work across them at once. It was scaffolded from
[agent-workspace-template](https://github.com/NickMoignard/agent-workspace-template).

## Getting started

Prerequisites (Homebrew, asdf, beads, `$AGENTS_GIT_SRC_DIR`) are provisioned by
agent-run skills. In order:

```bash
claude "/setup-homebrew, then /setup-asdf, then /setup-beads, then /setup-projects, then run make setup"
codex  "/setup-homebrew, then /setup-asdf, then /setup-beads, then /setup-projects, then run make setup"
```

## Projects

Referenced projects are listed in [`projects.yaml`](./projects.yaml) and linked
into `projects/`. Add one with:

```bash
make add-project URL=<git-url>
```

See [`CONTEXT-MAP.md`](./CONTEXT-MAP.md) for project contexts and relationships.

## Engineering playbook

**Every project referenced here follows the
[engineering playbook](docs/engineering-playbook/index.md)** — architecture,
development, contracts, operations, delivery, and reusable docs templates. The
defaults are a standalone React/Vite SPA, a TypeScript/Fastify BFF, and
capability-aligned TypeScript, Go, or Python services. A project that predates
the playbook converges on it: each divergence is recorded as a project ADR and
the remaining work is tracked in beads, so adoption is a direction of travel
rather than a claim that every project already complies.

Three local skills support it: `engineering-playbook` applies the standards,
`platform-slice` implements an authorized increment, and `platform-review`
checks compliance and evidence. The executable product template remains separate
implementation work; nothing in the playbook is a working command in this
parent workspace yet.

## Stack & skills

Toolchains pinned in [`.tool-versions`](./.tool-versions): python + uv, nodejs,
**pnpm**.

External agent skills (see [`skills-lock.json`](./skills-lock.json)):

- **[vercel-labs/agent-skills](https://github.com/vercel-labs/agent-skills)** —
  `vercel-react-best-practices`, `vercel-composition-patterns`,
  `vercel-react-view-transitions`, `vercel-optimize`, `deploy-to-vercel`,
  `vercel-cli-with-tokens`, `web-design-guidelines`, `writing-guidelines`.
- **[vercel-labs/agent-browser](https://github.com/vercel-labs/agent-browser)** —
  `agent-browser`, browser automation for checking the app in a real browser.
  The skill drives the separate `agent-browser` CLI; install it before first use.
- **[bmad-labs/skills](https://github.com/bmad-labs/skills)** —
  `typescript-clean-code`, `typescript-unit-testing`, `typescript-e2e-testing`.
- **[mattpocock/skills](https://github.com/mattpocock/skills)** — the full
  engineering + productivity collection (tdd, code-review, prototype, …).

Update them all with `make update-agent-skills`.

## More

See [`AGENTS.md`](./AGENTS.md) for the full working guide, `CONTEXT.md` /
`CONTEXT-MAP.md` for the domain glossary and context index, and `docs/adr/` for
the architectural decisions this workspace inherits.

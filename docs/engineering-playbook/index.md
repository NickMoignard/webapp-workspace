# Engineering playbook

The standard for standalone React SPAs, TypeScript BFFs, and capability-aligned TypeScript, Go, and Python services. Where a page states a default, follow it; record any departure as a project ADR. Where a page is unclear or silent on something you need, sharpen the page — do not settle it locally.

## Scope and status

This workspace owns the playbook and agent workflows. Product repositories own executable templates, applications, generators, CI, and their accompanying knowledge bases. **Every project referenced by this workspace follows this playbook** — adoption is not per-project opt-in, and a project added to `projects.yaml` adopts it. The parent workspace itself is not a product: its Make/asdf/beads conventions remain in force and are out of scope here.

Adoption is a direction of travel, not a claim of compliance. A project that predates the playbook keeps working while it converges: record each divergence as a project ADR with its intended resolution, and track the remaining work as beads issues. An unrecorded divergence is a gap, not an exemption. Use [platform-review](../../.agents/skills/platform-review/SKILL.md) to establish where a project actually stands.

**Current status: guidance integrated; executable platform acceptance not demonstrated.** Commands, paths, and acceptance gates below describe the intended product repository, not working commands in this parent workspace. Once implemented, executable examples are the source of truth for actual behavior; disagreements with approved intent require a fix or a recorded deviation, not silent changes to the rules.

## Find guidance

| Task | Canonical page |
| --- | --- |
| Choose a service boundary, design a BFF, enforce data ownership | [Architecture](architecture.md) |
| Build a React feature, TypeScript/Go/Python service, or root task | [Development](development.md) |
| Add endpoint, generated client, event, database migration, replay or backfill | [Contracts and data](contracts-and-data.md) |
| Local Kafka, authentication, authorization, trace request, production incident, deploy or rollback | [Operations and security](operations.md) |
| Implement a work package, test, generate scaffolds, establish CI | [Delivery](delivery.md) |
| Write a tutorial, service page, runbook, ADR or PR checklist | [Knowledge base and templates](knowledge-base.md) |

## Agent workflows

- [engineering-playbook](../../.agents/skills/engineering-playbook/SKILL.md): apply the relevant standards to a design or change.
- [platform-slice](../../.agents/skills/platform-slice/SKILL.md): implement an authorized vertical slice or generator increment.
- [platform-review](../../.agents/skills/platform-review/SKILL.md): assess architectural compliance and executable evidence.

Example requests: “Use engineering-playbook to design this BFF endpoint”; “Use platform-slice for WP-01 through WP-04 in this project”; “Use platform-review to assess this service against the playbook.”

These are workspace-owned skills, outside the external skills lockfile. Their relative links depend on this workspace layout; distribute the docs with them if moving them elsewhere. Imported React, NestJS, Jest, and deployment skills remain available where their framework and task match. Their tooling preferences do not replace these adopted project defaults.

## Maintenance

Change the canonical topic once and link to it from skills and project guidance. Each implemented architectural rule must link to an executable example, check, generator, or ADR in its product repository.

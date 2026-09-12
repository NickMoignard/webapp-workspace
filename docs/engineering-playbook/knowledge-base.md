# Knowledge base and reusable templates

## Information architecture

Product documentation lives beside executable code as plain Markdown and diagrams-as-code, readable without a replaceable site renderer. Create real pages as workflows land; empty scaffolds do not count as documentation.

| Directory | Intended pages |
| --- | --- |
| `docs/` | Outcome-oriented `index.md` |
| `start-here/` | prerequisites, create-a-project, run-the-example, first-change |
| `architecture/` | system-context, container-view, service-boundaries, bff, data-ownership |
| `development/` | repository, commands, frontend, typescript-services, go-services, python-services, contracts |
| `operations/` | local-development, observability, deployment, incident-runbook-template |
| `standards/` | testing, security, api-style, event-style, documentation |
| `adr/` | index/README and numbered decisions |
| `reference/` | generated configuration, ports, service-catalog; troubleshooting |

Every page declares `owner` and `last_reviewed` in frontmatter. Keep rules canonical and add links/context locally. Internal links fail CI when broken. Smoke-test tutorial commands where practical; check generated references for drift. New capabilities include docs in the same PR. Diagrams use actual component names and source-controlled text.

Search summaries should include the relevant developer vocabulary: create service, add endpoint/event, database migration, local Kafka, authentication/authorization, generated client, trace request, production incident, replay event, backfill, deploy, rollback. Use aliases where languages differ.

## Page contracts

| Type | Required content |
| --- | --- |
| Tutorial | Outcome, prerequisites, numbered steps, verification, cleanup, next step |
| How-to | Goal, assumptions, command/procedure, verification, failure paths |
| Explanation | Context, decision/model, trade-offs, ADR/example links |
| Reference | Authoritative inputs/outputs, examples, defaults, version behavior |
| Runbook | Trigger, impact, diagnosis, safe mitigation, recovery, escalation |
| ADR | Status, context, decision, alternatives, consequences, follow-ups |

The following fenced blocks are templates, not current project facts. Replace placeholders when instantiating them and include verified executable links.

## ADR template

```markdown
---
owner: <team/person>
last_reviewed: YYYY-MM-DD
---
# ADR NNNN: <decision>

Status: Proposed | Accepted | Superseded | Rejected
Date: YYYY-MM-DD

## Context
Forces and constraints.
## Decision
Enforceable consequences.
## Alternatives considered
Credible alternatives and why rejected.
## Consequences
Positive, negative, operational, migration, security effects.
## Follow-ups
Issue IDs, owners, completion signals.
```

## Service page template

```markdown
---
owner: <team>
last_reviewed: YYYY-MM-DD
---
# <service name>
Tier / repository path / runtime / deployable name

## Purpose and boundaries
## APIs and events
## Data ownership and retention
Classification, tenancy, backup/restore, deletion.
## Dependencies
## Configuration
## Local development
## Tests
## Deployment and migration
## Observability and SLOs
## Failure modes and runbook
## Decisions and known constraints
```

## How-to template

```markdown
---
owner: <team>
last_reviewed: YYYY-MM-DD
---
# How to <outcome>

## Goal
## Prerequisites and assumptions
## Procedure
Numbered commands and expected behavior.
## Verify
## Failure paths
## Clean up / rollback
## Related decisions and executable examples
```

## Runbook template

```markdown
---
owner: <team>
last_reviewed: YYYY-MM-DD
---
# <incident or operational outcome>

## Trigger and impact
Affected user outcome and owned capability.
## Signals and dependencies
Dashboard, logs, traces, metrics, SLOs, alerts; upstream/downstream,
data stores, topics, external providers.
## Diagnosis
Symptoms, confirmation queries, likely causes.
## Safe mitigation
Reversible rollback, scale, pause, replay or degradation; preconditions.
## Recovery
Validation, reconciliation, communication.
## Escalation
Primary/backup owners and decision thresholds.
```

## PR checklist

- Capability-aligned scope and intentional coupling.
- Browser/BFF/domain/data boundaries meet the [invariants](architecture.md#invariants).
- Additive contracts or explicit consumer migration plan.
- Typed/documented configuration and safe secrets handling.
- Adequate logs/traces/metrics and failure behavior.
- Tests at useful layers and passing relevant root checks.
- Current docs, generated artifacts and ADRs.

## Product agent instructions

When creating a product AGENTS.md, state its purpose (executable template and knowledge base), link its adopted architecture and task contracts, and require native pnpm/uv/Go dependency ownership, generated artifact discipline, BFF/domain/data boundaries, co-evolving docs/ADRs, `pnpm check` plus relevant integration/E2E evidence, and safe configuration. Keep rules in canonical project docs rather than copying this whole playbook into every instruction file. The parent workspace AGENTS.md continues to describe workspace management.

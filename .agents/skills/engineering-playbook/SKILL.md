---
name: engineering-playbook
description: "Apply the engineering playbook when designing or changing any project in this workspace: React SPA, TypeScript BFF, or TypeScript/Go/Python capability service. Covers architecture, state, contracts, data ownership, and operational standards; record divergence in a project ADR rather than silently migrating a working stack."
---

# Apply the engineering playbook

Read the [scope and index](../../../docs/engineering-playbook/index.md) and target repository instructions. Every project in this workspace follows the playbook, so the question is not whether it applies but how far the project has converged: check its ADRs for recorded divergences before assuming its current stack is a defect. Apply the playbook to the change at hand; surface conflicts and propose a recorded deviation or a tracked migration, and do not silently migrate frameworks or deployment targets as a side effect of an unrelated request.

Read the [architecture](../../../docs/engineering-playbook/architecture.md), then only references relevant to the change:

- React state/features, BFF structure, native services or Turbo tasks: [development](../../../docs/engineering-playbook/development.md).
- Endpoint/client generation, events, projections or migrations: [contracts and data](../../../docs/engineering-playbook/contracts-and-data.md).
- Health, identity, local infrastructure, telemetry or deployment: [operations](../../../docs/engineering-playbook/operations.md).
- Tests, CI, generators or work-package acceptance: [delivery](../../../docs/engineering-playbook/delivery.md).
- Tutorials, reference pages, runbooks or ADRs: [knowledge base](../../../docs/engineering-playbook/knowledge-base.md).

Apply the specific rules to the user's task. Prefer the repository's executable commands and inspect their implementations before relying on them. Where the playbook states a default, follow it; where it is silent, choose within the architecture authority rules and record material decisions. Ask only for a choice that blocks the authorized work. If a rule is genuinely unclear for the task at hand, fix the page rather than inventing a local reading.

Completion: explain the concrete change/design, applicable invariant IDs, executable validation or evidence still missing, and recorded deviations. Scope the evidence to the change; do not turn an ordinary feature request into full platform implementation or certification.

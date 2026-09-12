---
name: platform-review
description: "Review a project, proposed design, or change against the polyglot engineering playbook every workspace project follows, identifying architecture violations, unrecorded divergence, and missing executable acceptance evidence. Use when asked for playbook compliance or platform readiness; does not implement fixes unless requested."
---

# Review platform compliance

Read the [scope](../../../docs/engineering-playbook/index.md), [invariants](../../../docs/engineering-playbook/architecture.md) and target repository instructions/ADRs. Establish the requested review scope and how far the project has converged on the playbook — every workspace project is in scope, so a pre-playbook stack is reported as recorded deviation or unrecorded gap, not as exempt. For a diff, use the requested base and inspect changed callers/contracts; for a design, assess the proposal without pretending runnable evidence exists.

Read the relevant development, contracts/data, operations and knowledge-base pages linked from the index. Use [delivery](../../../docs/engineering-playbook/delivery.md) for the applicable phase's evidence. Report a violation against a stated rule; where the playbook says nothing, say so and recommend the rule rather than inventing one.

Trace one relevant path through the browser client, BFF, domain owner, persistence and events where present. Inspect contract sources, generated artifacts, task graph, tests and operational hooks as required by scope. Run relevant authorized checks where practical and record their actual outcomes. Missing evidence is unverified, not automatically a failing implementation. A configuration file alone does not prove behavior or CI success.

Report findings ordered by impact, each with:

- Concrete behavior, file/line or command evidence, and consequence.
- Canonical rule/invariant or acceptance gate it affects.
- Classification: violation, accepted ADR deviation, missing evidence, or unresolved design choice.
- Smallest corrective action and a way to verify it.

Conclude with checks run, material limits and whether the requested phase's acceptance is demonstrated. Keep recommendations scoped; a WP-01 review does not require WP-12 Kubernetes hardening. For generic code quality, existing review workflows remain available; this review adds playbook-specific evidence rather than replacing them.

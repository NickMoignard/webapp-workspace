---
name: platform-slice
description: "Implement an authorized vertical slice or generator increment in a workspace product under the polyglot engineering playbook. Use for platform work packages, create-project/create-app/create-service scaffolding, or a browser-to-BFF-to-capability feature; not for parent workspace setup."
---

# Implement a platform slice

Read the [playbook scope](../../../docs/engineering-playbook/index.md), [architecture](../../../docs/engineering-playbook/architecture.md), and [delivery protocol and roadmap](../../../docs/engineering-playbook/delivery.md). Inspect target instructions, existing code, worktree changes and relevant beads issue. If no product target is identifiable, ask for the target while reviewing independent requirements; do not create product code inside the parent workspace.

1. Map the authorized outcome to WP IDs and a short file-level plan. Use [development](../../../docs/engineering-playbook/development.md) for native tools/tasks and [delivery](../../../docs/engineering-playbook/delivery.md) for each WP's exit condition; meet that condition before claiming the scope is complete.
2. Implement the smallest complete behavior. For a new product's first slice, use the catalogue create/list journey described in delivery. For existing products, use the requested feature. Read [contracts/data](../../../docs/engineering-playbook/contracts-and-data.md) and [operations](../../../docs/engineering-playbook/operations.md) for affected boundaries; preserve domain/data ownership and include enough generation, infrastructure and telemetry to meet the slice's actual acceptance.
3. Validate meaningful batches using real repository commands. Repair failures before expanding. For generators, exercise fresh output in an isolated temporary target, its checks and golden tests, non-interactive flags, and existing-target failure behavior. Keep destructive reset operations restricted to explicitly selected project-owned local resources.
4. Update executable examples and [knowledge-base pages](../../../docs/engineering-playbook/knowledge-base.md) together. Record material deviations in project ADRs and remaining gaps in beads. Keep the repo runnable at each completed phase.

Hand off exact passing/failed/unrun commands, journey evidence, relevant contract/health/telemetry/container/shutdown results, deviations, gap issue IDs and next WP. Only claim acceptance actually demonstrated. A build request does not itself authorize publishing, production deployment, or service migration outside its scope.

---
name: afk-workflow
description: Deliver one authorized implementation ticket through independent agents and separate stage worktrees. Use for AFK delivery or the standard subagent worktree workflow.
---

# AFK worktree workflow

Coordinate one ticket through implementer → reviewer → review-fixer → merger.
Discovery selects this procedure; it grants neither ticket scope nor publication
authority. Resolve human decisions outside the pipeline before dispatching work.

## Prepare

1. Resolve the canonical repository path, ticket and requirements, acceptance
   criteria, applicable instructions and ADRs, target branch, remote, and the
   user's existing authority for implementation, commits and publication. Record
   the authorized delivery endpoint (for example, reviewed commit or pushed target).
   Readiness alone does not authorize a ticket. If a blocking decision or required
   authority is missing, prepare a concrete handoff and ask only for what is missing.
2. Inspect repository status and worktrees; fetch the target remote and pin its
   commit as the starting base. Record a unique run ID, canonical checkout status,
   resolved remote URL, target branch and initial review base. Use unique run-owned
   branches and worktree paths outside the canonical checkout.
3. Run `bd prime` in the [beads](https://github.com/gastownhall/beads) tracker, claim
   the ticket with `bd update --claim`, and record the run in beads. This workflow
   requires `bd`: beads notes are the durable handoff store every stage reads and writes. Confirm ownership before dispatch. Discover the
   available agent tools at runtime. Prefer distinct stage agents; for capacity
   constraints read [recovery](references/recovery.md).

Preparation is complete when scope, endpoint, ownership, base and required human
decisions are settled and recorded.

## Dispatch and gate

Send each stage the handoff below and its reference when it starts:

1. [Implementer](references/implementer.md): require a committed candidate and checks.
2. [Reviewer](references/reviewer.md): require independent assessments against the
   exact candidate and review base.
3. [Review-fixer](references/review-fixer.md): required findings enter the fix loop;
   otherwise pass through the approved commit. Every changed candidate returns to
   reviewer. Allow three fix-and-review rounds after initial review; persist the
   count across interruptions. Remaining required findings after round three block
   delivery: preserve the candidate and report them.
4. [Merger](references/merger.md): verify the authorized endpoint before closing.

For interruption, target movement, conflicts, capacity limits or cleanup, load
[recovery](references/recovery.md) only when needed.

## Handoff record

The coordinator writes each transition to the ticket's beads notes or comments;
workers return evidence for that record. Temporary logs supplement these durable
facts. Every dispatch and return includes:

- Ticket ID and pointers to canonical requirements, instructions, ADRs and gates.
- Run ID, repository, stage branch/worktree, remote, target and authorized endpoint.
- Starting base, current review base, candidate SHA and approval SHA/base (or pending).
- Agent roles and authorship, stage status, required findings with stable IDs,
  advisory observations, dispositions and unresolved decisions/limitations.
- Exact check commands, working directory, candidate SHA, outcome (passed, failed,
  unrun), and evidence location. Mark evidence fresh or cached; cached evidence
  names its original commit/environment and why it still applies. Record why any
  check was unrun. Mandatory missing evidence blocks approval.
- Fix-round and target-recovery counters, owned worktrees/processes, next action.

## Finish

Report the delivered commit and endpoint, Standards/Spec/platform outcomes,
verification limits, preserved work and next ticket (from `bd ready`, if available).
Starting that ticket, deploying, or publishing anything further requires its own
authorization.

## Companion skills

The stages invoke companion skills by role, not by path, so the pipeline runs with
whatever the repository has installed. Nothing here is bundled; the gating discipline
above holds regardless. Skills that fill these roles well:

- **Implementation standards** — a skill encoding the repository's architecture and
  conventions. Without one, the repository's own documented conventions are the standard.
- **Code review** — supplies the independent Standards and Spec axes the reviewer stage
  requires, for example
  [`code-review`](https://github.com/mattpocock/skills) (`npx skills@latest add mattpocock/skills`).
- **Platform/architecture review** — an extra axis against acceptance gates, if the
  repository defines one.
- **Merge conflicts** — used only by [recovery](references/recovery.md) on target movement,
  for example `resolving-merge-conflicts` from the same collection.

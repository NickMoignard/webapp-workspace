# Conditional recovery

## Target movement and conflicts

Allow three target-movement recovery attempts per run, counted separately from
fix-and-review rounds and persisted before each attempt. If movement requires a
fourth attempt, preserve work and report the blocker.

Fetch and pin the new remote target. Integrate it into the candidate in the
fixer's isolated branch/worktree, applying the repository's merge-conflict skill if
one is installed. Resolve routine conflicts there; send scope or design decisions to the coordinator
for human resolution outside the pipeline. Never resolve them in the canonical
checkout. Record the new review base, integration commit and affected checks.
Rerun affected checks, invalidate old approval, and repeat every review axis
against the new base/candidate. Required findings from that review use the same
run-wide fix-round budget. Even if integration leaves the candidate SHA unchanged,
a changed review base requires renewed review. Return to merger only with approval
for the current pair.

## Agent capacity

Prefer distinct implementer, reviewer, fixer and merger agents. Reuse threads
when capacity requires it, while preserving separate stage worktrees and recording
role assignments and the adaptation. Anyone who authored candidate changes may
implement/fix/merge but cannot approve that candidate.

Keep Standards and Spec assessors independent of authors and each other. If parallel
review slots are unavailable, dispatch them sequentially in distinct non-author
contexts. A coordinator who has authored no candidate changes may perform one
independent axis. Platform review may share a non-author reviewer. If available
contexts cannot preserve independent review, retain the committed handoff and
report that blocker instead of self-approving.

## Interrupted resumption

Read the ticket's durable run record before starting another stage. Reconcile its
SHAs, approvals, counters, claims and endpoint with actual branches, worktree
status, commit ancestry and current remote state. Inspect any uncommitted work;
retain it and determine ownership before continuing. Restore missing clean stage
worktrees from recorded commits where possible. Treat unreconciled approval or
check evidence as pending. Continue from the earliest unmet gate, preserving retry
counts; an interruption does not create a new retry budget.

A push may have succeeded before its record was written. Verify the actual remote
against the approved candidate before retrying publication or closing the ticket.
If the target moved, use the target-movement procedure above. Record a concrete
blocker when reconciliation needs unavailable evidence or a human decision.

## Preserve and clean up

Retain unfinished branches, commits, worktrees and evidence for a blocked run.
Remove only clean, completed run-owned worktrees after recording delivery and
handoff evidence. Inspect tracked and untracked files before ordinary `git worktree
remove`; use no force removal, hard reset, destructive clean or forced branch
removal. Stop only processes whose ownership by this run is established. Preserve
unrelated canonical checkout changes and other runs' work.

# Merger dispatch

Create a separate detached worktree at the approved commit. Verify the candidate
and review base match the approval, required findings are resolved, and mandatory
checks have applicable evidence. Run any required final checks and distinguish
fresh from cached evidence in the handoff.

Inspect the canonical checkout's status, branch and HEAD, all worktrees, and the
current target on the recorded remote (fetch it). Confirm the target is the
reviewed base and an ancestor of the approved candidate. If the target changed,
follow [recovery](recovery.md) before delivery. Preserve unrelated local state; if
the canonical target branch is dirty, divergent or held by another worktree, hand
off unless it can be advanced safely without disturbing another owner's work.

For a reviewed-commit endpoint, record the verified approved SHA without advancing
or publishing the target. For authorized merge-and-push delivery:

1. Fast-forward the local target to the approved candidate with `git merge
   --ff-only` in a clean checkout of that branch. Use the canonical checkout when
   it already owns the target; otherwise use a clean run-owned target checkout
   after confirming the local target has no unrelated commits. Keep merger
   verification in its separate worktree.
2. Push the target normally to the recorded remote and branch with an explicit
   refspec. Use no force option. On rejection, inspect remote state and enter
   target-movement recovery if applicable; other failures preserve work for resume.
3. Read the remote target SHA and verify it equals the approved candidate. If it
   moved again, reconcile through recovery rather than claiming delivery from the
   push command's exit code alone.

Complete only when the authorized endpoint is verified and recorded in beads;
then close the ticket. Return the shared handoff and remote verification evidence
when publication was authorized. Use [recovery](recovery.md) for safe cleanup.

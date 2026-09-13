# Review-fixer dispatch

Create a separate branch and worktree at the reviewed candidate. With no required
findings, return that unchanged SHA and its approval; create no empty commit.
Otherwise address each required finding, record its disposition and run affected
checks plus any gates invalidated by the changes. Apply the relevant implementation
skills. Commit the fixes and return the shared handoff record with the new SHA.

Complete when every required finding is addressed or explicitly blocked, checks
are recorded and intended fixes are committed in a clean worktree. Return every
changed candidate to the full reviewer stage, even for documentation-only fixes; the
previous approval does not apply to a new commit. The coordinator owns the round
budget in the entrypoint. For target integration use [recovery](recovery.md).

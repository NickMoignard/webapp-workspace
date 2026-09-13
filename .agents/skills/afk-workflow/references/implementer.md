# Implementer dispatch

Create a unique implementation branch and worktree from the pinned starting base.
Read the ticket's canonical requirements and repository instructions there. Apply
the repository's implementation-standards skill if one is installed, plus any skill
covering the authorized slice or generator. Follow their linked references rather
than substituting this workflow for implementation standards. Where no such skill
exists, the repository's own documented conventions are the standard; record which
standard you applied.

Implement only the authorized scope, update accompanying documentation, and run
the required checks. Commit the intended changes with the ticket reference. Keep
unrelated changes outside the commit. Return the shared handoff record, exact
candidate SHA, changed-file summary, check evidence and unresolved limitations.

Complete when the stage worktree is clean and the candidate is committed. A failed
or unavailable mandatory check must remain explicit for review, never reported as
passed. A new blocking human decision returns to the coordinator for resolution
outside the pipeline.

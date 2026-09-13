# Reviewer dispatch

Create a separate detached review worktree at the candidate SHA. Inspect the
committed candidate without editing tracked content. Review authors must be
independent of implementation and fixes for this candidate.

Assess the candidate on two independent axes against the exact review base and the
canonical ticket/spec: **Standards** (does it follow the repository's documented
conventions?) and **Spec** (does it do what the ticket asked?). Use the repository's
code-review skill for this if one is installed; its absence removes the tooling, not
the requirement that both axes be assessed independently. If the repository also
defines a platform or architecture review skill, apply it to the same diff and
applicable acceptance gates, and preserve its findings separately. For agent capacity
adaptations use [recovery](recovery.md).

Report required fixes separately from advisory observations. Each required finding
needs a stable ID, concrete evidence, governing requirement and verification
criterion. Missing mandatory acceptance evidence blocks approval; optional
limitations stay explicit without becoming invented gates. Run relevant checks or
identify the cached evidence used under the shared handoff contract.

Complete with an explicit approve or changes-required decision naming both exact
candidate SHA and review base, every review outcome (Standards, Spec, and platform
where it applies), and check evidence. Approval requires no outstanding required
findings and demonstrated mandatory acceptance. Verify the review worktree still
names the candidate and tracked files remain unchanged; a review summary without
SHA/base is not approval.

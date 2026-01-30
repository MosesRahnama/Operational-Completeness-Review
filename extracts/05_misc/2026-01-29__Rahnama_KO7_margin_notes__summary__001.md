Purpose: Evidence extract (misc/reference) documenting a failure or relevance; Duplicate of FRV2-0372.
Contents: Metadata header + excerpt from the source file.
Context: Author notes/tables; not a proof artifact.
Source: MUST_Review/important_2/Rahnama_KO7_margin_notes.md
SHA256: 645E5634F9BE6B1DDA293C66A413E40A1EDD4CA18A5930B4EB9A553BB60911EC
FailureExplanation: Author notes/tables; not a proof artifact.
FailureModeTags: 

Excerpt:
> # Red-pen margin notes for Rahnama_KO7.pdf
> ## 1. Fix rec-succ rule (remove duplication, add app)
> **Seen on pages:** 1, 2, 3, 4, 5, 6, 7, 8, 9, 10, 11
> **Replace:**
> > rec b s (succ n) → step (rec b s n) (rec b s n)
> **With:**
> > recΔ b s (delta n) → app s (recΔ b s n)
> **Comment:** Code does not duplicate RHS; strict decrease is δ-phase (1→0). Adjust orientation discussion accordingly.
> ## 2. Guard explicit: merge-cancel
> **Seen on pages:** 2, 3, 5, 6, 9, 10, 11
> **Replace:**
> > merge t t → t





Purpose: Evidence extract (misc/documentation) documenting a failure or relevance; Bridge file delegates ? lemmas to MetaSN/Termination (no new proofs).
Contents: Metadata header + excerpt from the source file.
Context: MuCore.lean bridge lemmas re-exporting MetaSN results (no new proofs of core bounds).
Source: MUST_Review/Legacy/Meta_Lean_Archive/MuCore.lean
SHA256: 4A663976D673AEA17AE4C6DEF5B61874386865E5AD89B4705A25B82D9C85F20F
FailureExplanation: This file primarily re-exports and delegates key μ lemmas to MetaSN/Termination without new justification.
FailureModeTags: 

Excerpt:
> MetaSN.termB_le x
>
> /-- **Key μ‑drop for `eqW_diff`** (independent of the `a ≠ b` premise):
>     `μ (integrate (merge a b)) < μ (eqW a b)`. -/
> theorem mu_lt_eq_diff (a b : Trace) :
>   MetaSN.mu (.integrate (.merge a b)) < MetaSN.mu (.eqW a b) :=
>   -- Just use the working theorem from Termination.lean
>   MetaSN.mu_lt_eq_diff a b
>
> /-- local strict-mono bridge: if `a < b` then `ω^a < ω^b` -/
> private lemma opow_lt_opow_right {a b : Ordinal} (h : a < b) :
>     omega0 ^ a < omega0 ^ b := by
>   -- `isNormal_opow` supplies strict monotonicity for `ω`-powers
>   simpa using ((isNormal_opow (a := omega0) one_lt_omega0).strictMono h)
>
> end MuCore





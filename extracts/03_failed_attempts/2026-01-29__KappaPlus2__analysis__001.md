Purpose: Evidence extract (failed_attempts/analysis) documenting a failure or relevance; Duplicate of MUST_Review\Legacy\Meta_Lean_Archive\KappaPlus2.lean.
Contents: Metadata header + excerpt from the source file.
Context: Excerpt from KappaPlus2.lean redefining κ with a +2 bump on recΔ δ-case and proving strict drop for rec-succ.
Source: MUST_Review/Legacy/Meta_Lean_Archive/KappaPlus2.lean
SHA256: 78D6644CB5C567A2A4355E9145CE45F6E5A6D73DE974A010445DDC9CE4CA9A30
FailureExplanation: The measure is altered with an ad hoc +2 bump to force a strict drop, changing the intended invariant rather than resolving the underlying recursion/duplication issue.
FailureModeTags: nondecreasing_measure

Excerpt:
> import OperatorKernelO6.Kernel
> import Init.WF
> import Mathlib.Data.Prod.Lex
> import Mathlib.SetTheory.Ordinal.Basic
>
> namespace KappaPlus2
>
> open OperatorKernelO6 Trace Ordinal Nat
> def kappa : Trace → Nat
> | .void => 0
> | .delta t => kappa t
> | .integrate t => kappa t
> | .merge a b => Nat.max (kappa a) (kappa b)
> | .recΔ b s (.delta n) =>
>     Nat.max (Nat.max (kappa b) (kappa s)) (kappa n) + 2
> | .recΔ b s n =>
>     Nat.max (Nat.max (kappa b) (kappa s)) (kappa n)
> | .eqW a b => Nat.max (kappa a) (kappa b)   -- TWO args here
>
> @[simp] theorem kappa_rec_base (b s n) :
>   kappa (recΔ b s n) =
>     Nat.max (Nat.max (kappa b) (kappa s)) (kappa n) := by
>   cases n <;> simp [kappa]
>
> @[simp] theorem kappa_rec_succ  (b s n) :
>   kappa (recΔ b s (delta n)) =
>     Nat.max (Nat.max (kappa b) (kappa s)) (kappa n) + 2 := rfl
>
> /-- Strict κ drop for successor reduction. -/
> lemma drop_recSucc (b s n : Trace) :
>   kappa (app s (recΔ b s n)) < kappa (recΔ b s (delta n)) := by
>   let base : Nat := max (max (kappa b) (kappa s)) (kappa n)
>
>   -- κ(rec) ≤ base + 2   (note the +2, not +1)
>   have h_rec : kappa (recΔ b s n) ≤ base + 2 := by
>     cases n with
>     | delta _ => simp [kappa, base, add_comm, add_left_comm]
>     | _       => simp [kappa, base, le_rfl]
>
>   -- κ(merge) ≤ base + 2
>   have h_merge : kappa (app s (recΔ b s n)) ≤ base + 2 := by
>     have h_s : kappa s ≤ base :=
>       (le_max_right _ _).trans (le_max_left _ _)
>     have : max (kappa s) (kappa (recΔ b s n)) ≤ base + 2 :=
>       max_le (le_trans h_s (Nat.le_trans (le_refl _) (le_add_of_nonneg_right (zero_le 2)))) h_rec
>     simpa [kappa] using this
>
>   -- RHS = base + 3
>   have h_rhs : kappa (recΔ b s (delta n)) = base + 3 := by
>     simp [kappa_rec_succ, base]
>
>   have : kappa (app s (recΔ b s n)) < base + 3 :=
>     lt_of_le_of_lt h_merge (lt_succ_self _)
>   simpa [h_rhs] using this






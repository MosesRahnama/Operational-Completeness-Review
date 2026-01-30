Purpose: Evidence extract (failed_attempts/analysis) documenting a failure or relevance; Kappa definition increments for every rec?, violating the intended ? drop-only-on-rec_succ invariant.
Contents: Metadata header + excerpt from the source file.
Context: Kappa.lean excerpt; kappa counts all recΔ (not only recΔ delta-case).
Source: MUST_Review/MetaMD_Archive/Kappa.md
SHA256: DDA891BE8336D7956783DA77E91014D9F49D3F153BBCC930367E7CAC4D53E8E6
FailureExplanation: Kappa definition increments for every recΔ, violating the intended κ drop-only-on-rec_succ invariant.
FailureModeTags: nondecreasing_measure; constraint_violation

Excerpt:
> def kappa : Trace → Nat
> | .void => 0
> | .delta t => kappa t
> | .integrate t => kappa t
> | .merge a b => max (kappa a) (kappa b)
> | .recΔ b s n => 1 + max (kappa b) (max (kappa s) (kappa n))
> | .eqW a b => max (kappa a) (kappa b)
>
> -- Lemmas about κ for each Step rule
>
> @[simp] theorem kappa_le_integrate_delta (t : Trace) : kappa .void ≤ kappa (.integrate (.delta t)) := by
>   simp [kappa, Nat.zero_le]
>
> @[simp] theorem kappa_eq_merge_void_left (t : Trace) : kappa (.merge .void t) = kappa t := by
>   simp [kappa]
>
> @[simp] theorem kappa_eq_merge_void_right (t : Trace) : kappa (.merge t .void) = kappa t := by
>   simp [kappa]
>
> @[simp] theorem kappa_eq_merge_cancel (t : Trace) : kappa (.merge t t) = kappa t := by
>   simp [kappa]
>
> @[simp] theorem kappa_lt_rec_zero (b s : Trace) : kappa b < kappa (.recΔ b s .void) := by
>   simp only [kappa]
>   -- Goal: kappa b < 1 + max (kappa b) (max (kappa s) 0)
>   rw [← Nat.succ_eq_one_add] -- Explicitly change 1 + m to m.succ
>   apply Nat.lt_succ_of_le
>   -- Goal: kappa b ≤ max (kappa b) (max (kappa s) 0)
>   exact Nat.le_max_left _ _
>
> @[simp] theorem kappa_eq_rec_succ (b s n : Trace) :
>     kappa (app s (recΔ b s n)) = kappa (recΔ b s (delta n)) := by
>   simp only [kappa] -- Unfold kappa on both sides
>   apply Nat.max_eq_right
>   -- Goal: kappa s ≤ 1 + max (kappa b) (max (kappa s) (kappa n))
>   rw [← Nat.succ_eq_one_add] -- Explicitly change 1 + m to m.succ
>   apply Nat.le_succ_of_le
>   exact Nat.le_trans (Nat.le_max_left _ _) (Nat.le_max_right _ _)
>
> @[simp] theorem kappa_le_eq_refl (a : Trace) : kappa .void ≤ kappa (eqW a a) := by
>   simp [kappa, Nat.zero_le]
>
> @[simp] theorem kappa_eq_eq_diff (a b : Trace) :
>     kappa (integrate (merge a b)) = kappa (eqW a b) := by
>   simp [kappa]






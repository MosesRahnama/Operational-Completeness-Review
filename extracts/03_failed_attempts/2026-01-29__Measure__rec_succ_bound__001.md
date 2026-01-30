Purpose: Evidence extract (failed_attempts/analysis) documenting a failure or relevance; Rec_succ delta-case depends on MetaSN.rec_succ_bound (unsupported), leaving a hard blocker.
Contents: Metadata header + excerpt from the source file.
Context: Measure.lean copy with rec_succ_bound dependency in rec_succ delta case.
Source: MUST_Review/MetaMD_Archive/Measure.md
SHA256: EDC3A322668F09A5FA6922E94666D18911174E37C274AFBA7E7AB9560A889B3A
FailureExplanation: Rec_succ delta-case depends on MetaSN.rec_succ_bound (unsupported), leaving a hard blocker.
FailureModeTags: unsupported_inference; nonconvergence

Excerpt:
> | _, _, Step.R_rec_succ b s n => by
>     -- Split on n. If n = delta m: κ equal, use μ-drop; else κ drops by 1.
>     cases n with
>     | delta m =>
>         -- κ equality against LHS: both sides compute to base+1 where base := max (max κ b κ s) κ m
>         have hkL : kappa (recΔ b s (delta (delta m))) =
>           Nat.max (Nat.max (kappa b) (kappa s)) (kappa m) + 1 := by
>           simp [kappa]
>         have hkR : kappa (merge s (recΔ b s (delta m))) =
>           Nat.max (Nat.max (kappa b) (kappa s)) (kappa m) + 1 := by
>           have hs_le : kappa s ≤ Nat.max (Nat.max (kappa b) (kappa s)) (kappa m) :=
>             le_trans (Nat.le_max_right _ _) (Nat.le_max_left _ _)
>           have : kappa (recΔ b s (delta m)) = Nat.max (Nat.max (kappa b) (kappa s)) (kappa m) + 1 := by
>             simp [kappa]
>           simpa [kappa_merge, this, Nat.max_eq_right (Nat.le_trans hs_le (Nat.le_succ _))]
>         have hk : kappa (merge s (recΔ b s (delta m))) = kappa (recΔ b s (delta (delta m))) := by
>           simpa [hkL, hkR]
>         -- μ-drop from Legacy on (delta m)
>   have hμ := MetaSN.mu_lt_rec_succ b s (delta m) (MetaSN.rec_succ_bound b s (delta m))
>         exact drop_right hμ hk
>     | void =>
>         let base := Nat.max (Nat.max (kappa b) (kappa s)) (kappa void)
>         have hs_le : kappa s ≤ base :=
>           le_trans (Nat.le_max_right _ _) (Nat.le_max_left _ _)
>         have h_rhs : kappa (merge s (recΔ b s void)) = base := by
>           have hrecd : kappa (recΔ b s void) = base := by simp [kappa, base]
>           simpa [kappa_merge, hrecd, Nat.max_eq_right hs_le]
>         have h_lhs : kappa (recΔ b s (delta void)) = base + 1 := by
>           simp [kappa_rec_delta, base, kappa_void]
>         exact drop_left (by simpa [h_rhs, h_lhs] using Nat.lt_succ_self base)
>     | integrate t =>
>         let base := Nat.max (Nat.max (kappa b) (kappa s)) (kappa t)
>         have hs_le : kappa s ≤ base :=
>           le_trans (Nat.le_max_right _ _) (Nat.le_max_left _ _)
>         have h_rhs : kappa (merge s (recΔ b s (integrate t))) = base := by
>           have hrecd : kappa (recΔ b s (integrate t)) = base := by simp [kappa, base]
>           simpa [kappa_merge, hrecd, Nat.max_eq_right hs_le]
>         have h_lhs : kappa (recΔ b s (delta (integrate t))) = base + 1 := by
>           simp [kappa_rec_delta, base]
>         exact drop_left (by simpa [h_rhs, h_lhs] using Nat.lt_succ_self base)
>     | merge a c =>
>         let base := Nat.max (Nat.max (kappa b) (kappa s)) (Nat.max (kappa a) (kappa c))
>         have hs_le : kappa s ≤ base :=
>           le_trans (Nat.le_max_right _ _) (Nat.le_max_left _ _)
>         have h_rhs : kappa (merge s (recΔ b s (merge a c))) = base := by
>           have hrecd : kappa (recΔ b s (merge a c)) = base := by simp [kappa, base]
>           simpa [kappa_merge, hrecd, Nat.max_eq_right hs_le]
>         have h_lhs : kappa (recΔ b s (delta (merge a c))) = base + 1 := by
>           simp [kappa_rec_delta, base]
>         exact drop_left (by simpa [h_rhs, h_lhs] using Nat.lt_succ_self base)
>     | recΔ b' s' n' =>
>         let base := Nat.max (Nat.max (kappa b) (kappa s)) (Nat.max (Nat.max (kappa b') (kappa s')) (kappa n'))
>         have hs_le : kappa s ≤ base :=
>           le_trans (Nat.le_max_right _ _) (Nat.le_max_left _ _)
>         have h_inner : kappa (recΔ b' s' n') = Nat.max (Nat.max (kappa b') (kappa s')) (kappa n') := by
>           simp [kappa]
>         have h_rhs : kappa (merge s (recΔ b s (recΔ b' s' n'))) = base := by
>           have hrecd : kappa (recΔ b s (recΔ b' s' n')) = base := by
>             simp [kappa, base, h_inner]
>           simpa [kappa_merge, hrecd, Nat.max_eq_right hs_le]
>         have h_lhs : kappa (recΔ b s (delta (recΔ b' s' n'))) = base + 1 := by
>           simp [kappa_rec_delta, base, h_inner]
>         exact drop_left (by simpa [h_rhs, h_lhs] using Nat.lt_succ_self base)
>     | eqW a c =>
>         let base := Nat.max (Nat.max (kappa b) (kappa s)) (Nat.max (kappa a) (kappa c))
>         have hs_le : kappa s ≤ base :=
>           le_trans (Nat.le_max_right _ _) (Nat.le_max_left _ _)
>         have h_rhs : kappa (merge s (recΔ b s (eqW a c))) = base := by
>           have hrecd : kappa (recΔ b s (eqW a c)) = base := by simp [kappa, base]
>           simpa [kappa_merge, hrecd, Nat.max_eq_right hs_le]
>         have h_lhs : kappa (recΔ b s (delta (eqW a c))) = base + 1 := by
>           simp [kappa_rec_delta, base]
>         exact drop_left (by simpa [h_rhs, h_lhs] using Nat.lt_succ_self base)





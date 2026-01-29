Context: Excerpt from Measure.md showing the lexicographic measure proof and its reliance on rec_succ_bound in the rec_succ delta case.
Source: C:\Users\Moses\OpComp\MUST_Review\Legacy\MetaMD_Archive\Measure.md
SHA256: EDC3A322668F09A5FA6922E94666D18911174E37C274AFBA7E7AB9560A889B3A
FailureExplanation: The rec_succ case depends on MetaSN.rec_succ_bound, a disallowed domination assumption, so the lex measure proof is invalid under project constraints.
FailureModeTags: constraint_violation; unsupported_inference

Excerpt:
> ```lean
> 
> import OperatorKernelO6.Kernel
> import OperatorKernelO6.Meta.Termination
> import Init.WF
> import Mathlib.Data.Prod.Lex
> -- import OperatorKernelO6.Meta.MuCore  -- use MuCore.mu &
> 
> set_option linter.unnecessarySimpa false
> open OperatorKernelO6 Trace Ordinal MetaSN
> 
> namespace SN
> 
> /-- Canonical recursion-depth κ: max-based; +1 only at `recΔ _ _ (delta _)`. -/
> def kappa : Trace → Nat
> | .void                => 0
> | .delta t             => kappa t
> | .integrate t         => kappa t
> | .merge a b           => Nat.max (kappa a) (kappa b)
> | .recΔ b s (.delta n) => Nat.max (Nat.max (kappa b) (kappa s)) (kappa n) + 1
> | .recΔ b s n          => Nat.max (Nat.max (kappa b) (kappa s)) (kappa n)
> | .eqW a b             => Nat.max (kappa a) (kappa b)
> 
> @[simp] lemma kappa_void : kappa void = 0 := rfl
> @[simp] lemma kappa_delta (t) : kappa (delta t) = kappa t := rfl
> @[simp] lemma kappa_integrate (t) : kappa (integrate t) = kappa t := rfl
> @[simp] lemma kappa_merge (a b) : kappa (merge a b) = Nat.max (kappa a) (kappa b) := rfl
> @[simp] lemma kappa_eqW (a b) : kappa (eqW a b) = Nat.max (kappa a) (kappa b) := rfl
> @[simp] lemma kappa_rec_delta (b s n) :
>   kappa (recΔ b s (delta n)) =
>     Nat.max (Nat.max (kappa b) (kappa s)) (kappa n) + 1 := rfl
> 
> /-- Combined lexicographic measure (κ, μ) using MetaSN.mu. -/
> noncomputable def measure (t : Trace) : Nat × Ordinal := (kappa t, MetaSN.mu t)
> 
> /-- Lexicographic order on Nat × Ordinal. -/
> def LexOrder : (Nat × Ordinal) → (Nat × Ordinal) → Prop :=
>   Prod.Lex (· < ·) (· < ·)
> 
> /-- The lexicographic order is well-founded. -/
> lemma wf_LexOrder : WellFounded LexOrder := by
>   exact WellFounded.prod_lex Nat.lt_wfRel.wf Ordinal.lt_wf
> 
> /-- When κ drops strictly, we get lexicographic decrease. -/
> lemma drop_left {a b : Trace} (hk : kappa b < kappa a) :
>     LexOrder (measure b) (measure a) := by
>   unfold LexOrder measure
>   exact Prod.Lex.left _ _ hk
> 
> /-- When κ is equal and μ drops strictly, we get lexicographic decrease. -/
> lemma drop_right {a b : Trace} (hμ : MetaSN.mu b < MetaSN.mu a) (hk : kappa b = kappa a) :
>     LexOrder (measure b) (measure a) := by
>   unfold LexOrder measure
>   rw [hk]
>   exact Prod.Lex.right _ hμ
> 
> /-- Every primitive step strictly decreases the lexicographic measure. -/
> theorem measure_decreases : ∀ {a b : Trace}, Step a b → LexOrder (measure b) (measure a)
> | _, _, Step.R_int_delta t => by
>     by_cases h : kappa t = 0
>     · apply drop_right (MetaSN.mu_void_lt_integrate_delta t)
>       simpa [kappa_integrate, kappa_delta, kappa_void, h]
>     · have hpos : 0 < kappa t := Nat.pos_of_ne_zero h
>       apply drop_left
>       simpa [kappa_integrate, kappa_delta, kappa_void] using hpos
> 
> | _, _, Step.R_merge_void_left t => by
>     apply drop_right (MetaSN.mu_lt_merge_void_left t)
>     simpa [kappa_merge, kappa_void, Nat.max_eq_right (Nat.zero_le _)]
> 
> | _, _, Step.R_merge_void_right t => by
>     apply drop_right (MetaSN.mu_lt_merge_void_right t)
>     simpa [kappa_merge, kappa_void, Nat.max_eq_left (Nat.zero_le _)]
> 
> | _, _, Step.R_merge_cancel t => by
>     apply drop_right (MetaSN.mu_lt_merge_cancel t)
>     simpa [kappa_merge, Nat.max_self]
> 
> | _, _, Step.R_rec_zero b s => by
>     have hb_le : kappa b ≤ kappa (recΔ b s void) := by
>       have h1 : kappa b ≤ Nat.max (kappa b) (kappa s) := Nat.le_max_left _ _
>       have h2 : Nat.max (kappa b) (kappa s) ≤
>                 Nat.max (Nat.max (kappa b) (kappa s)) (kappa void) := Nat.le_max_left _ _
>       simpa [kappa, kappa_void] using le_trans h1 h2
>     by_cases hb_eq : kappa b = kappa (recΔ b s void)
>     · exact drop_right (MetaSN.mu_lt_rec_base b s) hb_eq
>     · exact drop_left (Nat.lt_of_le_of_ne hb_le hb_eq)
> 
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

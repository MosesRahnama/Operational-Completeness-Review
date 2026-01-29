Context: StrongNormal.lean reuses kappaD and MetaSN μ lemmas to claim lexicographic decrease.
Source: C:\Users\Moses\OpComp\MUST_Review\Legacy\Meta_Lean_Archive\StrongNormal.lean
SHA256: CD683DF3F7DB90549E7DF5E680D21EE7D52596FCE75A68D5F964D4DBA3702FAC
FailureExplanation: Relies on imported μ-drop lemmas and kappaD_drop_recSucc without independent justification.
FailureModeTags: unsupported_inference

Excerpt:
> /-- Every primitive step strictly decreases the lexicographic measure. -/
> theorem measure_decreases : ∀ {a b : Trace}, Step a b → LexOrder (measure b) (measure a)
> | _, _, R_int_delta t =>
>     -- κ equal; use μ(void) < μ(integrate (delta t))
>     drop_right (MetaSN.mu_void_lt_integrate_delta t) (by simp [measure, kappa])
> | _, _, R_merge_void_left t =>
>     drop_right (MetaSN.mu_lt_merge_void_left t) (by simp [kappa])
> | _, _, R_merge_void_right t =>
>     drop_right (MetaSN.mu_lt_merge_void_right t) (by simp [kappa])
> | _, _, R_merge_cancel t =>
>     drop_right (MetaSN.mu_lt_merge_cancel t) (by simp [kappa])
> | _, _, R_rec_zero b s =>
>     -- κ(b) ≤ κ(recΔ b s void); if equal, use μ-drop; else κ-drop
>     by
>       have hb_le : kappa b ≤ kappa (recΔ b s void) := by
>         have h1 : kappa b ≤ Nat.max (kappa b) (kappa s) := Nat.le_max_left _ _
>         have h2 : Nat.max (kappa b) (kappa s) ≤
>                   Nat.max (Nat.max (kappa b) (kappa s)) (kappa void) := Nat.le_max_left _ _
>         simpa [kappa] using le_trans h1 h2
>       by_cases hb_eq : kappa b = kappa (recΔ b s void)
>       · exact drop_right (MetaSN.mu_lt_rec_zero b s) hb_eq
>       · exact drop_left (Nat.lt_of_le_of_ne hb_le hb_eq)
> | _, _, R_rec_succ b s n =>
>     -- single line: κ strictly drops for successor step
>     drop_left (kappa_drop_recSucc b s n)
> | _, _, R_eq_refl a =>
>     -- κ equal (to max κ a κ a); μ(void) < μ(eqW a a)
>     drop_right (MetaSN.mu_void_lt_eq_refl a) (by simp [kappa])
> | _, _, R_eq_diff (a:=a) (b:=b) hneq =>
>     -- κ equal; μ strictly drops via the eq-diff lemma
>     drop_right (MetaSN.mu_lt_eq_diff a b) (by simp [kappa])
>
> /-- Reverse relation for forward normalization. -/

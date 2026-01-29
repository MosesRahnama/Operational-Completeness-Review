Context: Excerpt from StrongNormal showing the kappaD lex measure and reliance on μ lemmas for step decreases.
Source: C:\Users\Moses\OpComp\MUST_Review\Legacy\MetaMD_Archive\StrongNormal.md
SHA256: 8E13E399EF025C737B4AC93A3035FE57F5B7CE1E7051A0F2C1221B6A5F0E6B85
FailureExplanation: The proof depends on μ lemmas (e.g., mu_lt_eq_diff, mu_lt_rec_zero) that are missing or disputed elsewhere, so SN is not established.
FailureModeTags: proof_obligation_stuck

Excerpt:
> /-- Structural measure κᴰ (bumps by +1 exactly at `recΔ _ _ (delta _)`). -/
> def kappaD : Trace → Nat
> | .void                => 0
> | .delta t             => kappaD t
> | .integrate t         => kappaD t
> | .merge a b           => Nat.max (kappaD a) (kappaD b)
> | .recΔ b s (.delta n) => Nat.max (Nat.max (kappaD b) (kappaD s)) (kappaD n) + 1
> | .recΔ b s n          => Nat.max (Nat.max (kappaD b) (kappaD s)) (kappaD n)
> | .eqW a b             => Nat.max (kappaD a) (kappaD b)
> 
> @[simp] lemma kappaD_void : kappaD .void = 0 := rfl
> @[simp] lemma kappaD_delta (t) : kappaD (.delta t) = kappaD t := rfl
> @[simp] lemma kappaD_integrate (t) : kappaD (.integrate t) = kappaD t := rfl
> @[simp] lemma kappaD_merge (a b) : kappaD (.merge a b) = Nat.max (kappaD a) (kappaD b) := rfl
> @[simp] lemma kappaD_eqW (a b)   : kappaD (.eqW a b)   = Nat.max (kappaD a) (kappaD b) := rfl
> @[simp] lemma kappaD_rec_succ (b s n) :
>   kappaD (.recΔ b s (.delta n)) =
>     Nat.max (Nat.max (kappaD b) (kappaD s)) (kappaD n) + 1 := rfl
> @[simp] lemma kappaD_rec_base (b s n) :
>   kappaD (.recΔ b s n) =
>     Nat.max (Nat.max (kappaD b) (kappaD s)) (kappaD n) := by
>   cases n <;> simp [kappaD]
> 
> /-- Combined measure `(κ, μ)` (mark noncomputable because `μ` is). -/
> noncomputable def muHat (t : Trace) : Nat × Ordinal := (kappaD t, mu t)
> 
> /-- Lex on `(Nat × Ordinal)`. -/
> def LexNatOrd : (Nat × Ordinal) → (Nat × Ordinal) → Prop :=
>   Prod.Lex (· < ·) (· < ·)
> 
> /-- WF of the lex product (`Nat.<` × `Ordinal.<`). -/
> lemma wf_LexNatOrd : WellFounded LexNatOrd :=
>   WellFounded.prod_lex Nat.lt_wfRel.wf Ordinal.lt_wf
> 
> /-- Left branch: κ strictly drops. -/
> @[inline] lemma drop_left {a b : Trace}
>   (hk : kappaD b < kappaD a) : LexNatOrd (muHat b) (muHat a) := by
>   unfold LexNatOrd muHat; exact Prod.Lex.left _ _ hk
> 
> /-- Right branch: κ equal, μ strictly drops. -/
> @[inline] lemma drop_right {a b : Trace}
>   (hμ : mu b < mu a) (hk : kappaD b = kappaD a) :
>   LexNatOrd (muHat b) (muHat a) := by
>   unfold LexNatOrd muHat; cases hk
>   simpa using (Prod.Lex.right (r₁ := (· < ·)) (r₂ := (· < ·)) hμ)
> 
> /-- κ strictly drops on the recursion successor step (all shapes). -/
> lemma kappaD_drop_recSucc (b s n : Trace) :
>   kappaD (.merge s (.recΔ b s n)) < kappaD (.recΔ b s (.delta n)) := by
>   -- Let M be the `max` base on (b,s,n).
>   let M := Nat.max (Nat.max (kappaD b) (kappaD s)) (kappaD n)
>   have hs_le_M : kappaD s ≤ M :=
>     Nat.le_trans (Nat.le_max_right _ _) (Nat.le_max_left _ _)
>   have hmax : Nat.max (kappaD s) M = M := Nat.max_eq_right hs_le_M
>   calc
>     kappaD (.merge s (.recΔ b s n))
>         = Nat.max (kappaD s) (kappaD (.recΔ b s n)) := by simp [kappaD]
>     _   = Nat.max (kappaD s) M := by simp [kappaD_rec_base, M]
>     _   = M := by simpa [hmax]
>     _   < M + 1 := Nat.lt_succ_self _
>     _   = kappaD (.recΔ b s (.delta n)) := by simp [kappaD_rec_succ, M]
> 
> /-- Each primitive step strictly decreases `(κ, μ)` in lexicographic order. -/
> lemma measure_drop_of_step :
>   ∀ {a b : Trace}, Step a b → LexNatOrd (muHat b) (muHat a)
> | _, _, Step.R_int_delta t => by
>     -- κ(void)=0; κ(source)=κ t. If κ t = 0 use μ, else drop-left.
>     have hμ := mu_void_lt_integrate_delta t
>     by_cases ht0 : kappaD t = 0
>     · have hk : kappaD (.void) = kappaD (.integrate (.delta t)) := by simp [kappaD, ht0]
>       exact drop_right hμ hk.symm
>     · have hk : kappaD (.void) < kappaD (.integrate (.delta t)) := by
>         have : 0 < kappaD t := Nat.pos_of_ne_zero ht0
>         simpa [kappaD] using this
>       exact drop_left hk
> 
> | _, _, Step.R_merge_void_left t => by
>     have hμ := mu_lt_merge_void_left t
>     have hk : kappaD t = kappaD (.merge .void t) := by simp [kappaD]
>     exact drop_right hμ hk
> 
> | _, _, Step.R_merge_void_right t => by
>     have hμ := mu_lt_merge_void_right t
>     have hk : kappaD t = kappaD (.merge t .void) := by simp [kappaD]
>     exact drop_right hμ hk
> 
> | _, _, Step.R_merge_cancel t => by
>     have hμ := mu_lt_merge_cancel t
>     have hk : kappaD t = kappaD (.merge t t) := by simp [kappaD, Nat.max_idem]
>     exact drop_right hμ hk
> 
> | _, _, Step.R_rec_zero b s => by
>     have hμ := mu_lt_rec_zero b s
>     -- κ(rec b s void) = max (max κb κs) 0 ≥ κ b
>     have hle : kappaD b ≤ kappaD (.recΔ b s .void) := by
>       simp [kappaD, Nat.le_max_left]
>     by_cases hEq : kappaD b = kappaD (.recΔ b s .void)
>     · exact drop_right hμ hEq
>     · exact drop_left (Nat.lt_of_le_of_ne hle hEq)
> 
> | _, _, Step.R_rec_succ b s n => by
>     -- recΔ b s (delta n) → merge s (recΔ b s n)  (κ drops by 1)
>     exact drop_left (kappaD_drop_recSucc b s n)
> 
> | _, _, Step.R_eq_refl a => by
>     have hμ := mu_void_lt_eq_refl a
>     by_cases h0 : kappaD a = 0
>     · have hk : kappaD (.void) = kappaD (.eqW a a) := by simp [kappaD, h0]
>       exact drop_right hμ hk
>     · have hk : kappaD (.void) < kappaD (.eqW a a) := by
>         have : 0 < kappaD a := Nat.pos_of_ne_zero h0
>         simpa [kappaD, Nat.max_self] using this
>       exact drop_left hk
> 
> | a, b, Step.R_eq_diff hneq => by
>     -- eqW a b → integrate (merge a b)  (κ equal; μ drops)
>     have hμ := mu_lt_eq_diff a b
>     have hk : kappaD (.eqW a b) = kappaD (.integrate (.merge a b)) := by
>       simp [kappaD, Nat.max_assoc, Nat.max_comm, Nat.max_left_comm]
>     exact drop_right hμ hk

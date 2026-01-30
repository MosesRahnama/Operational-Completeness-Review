Purpose: Evidence extract (failed_attempts/proof-attempt) documenting a failure or relevance; Duplicate of Legacy\\MetaMD_Archive\\SN_Final.md.
Contents: Metadata header + excerpt from the source file.
Context: Excerpt from SN_Final showing the deltaFlag/kappa lex measure setup and measure_decreases skeleton.
Source: Legacy/MetaMD_Archive/SN_Final.md
SHA256: 55B2AD5600522515D9AA51729D5628CE39504DF5DD36B235B773F3F045BFB180
FailureExplanation: The proof claims deltaFlag/kappa lex decreases, but diagnostics show rec_succ stuck and pattern errors; the measure strategy is unvalidated.
FailureModeTags: unsupported_inference; nondecreasing_measure

Excerpt:
> namespace SN
> 
> /-- Primary flag: `1` when the **root** term is of the form `recΔ _ _ (delta _)`, otherwise `0`.
>     This is used as the first component in the triple lexicographic measure to
>     guarantee a strict decrease for the `R_rec_succ` rule. -/
> @[simp] def deltaFlag : Trace → Nat
>   | .recΔ _ _ (.delta _) => 1
>   | _                    => 0
> 
> @[simp] lemma deltaFlag_merge  (a b) : deltaFlag (merge a b) = 0 := by
>   cases a <;> simp [merge, deltaFlag]
> 
> @[simp] lemma deltaFlag_recSucc_left  (b s n) :
>   deltaFlag (recΔ b s (delta n)) = 1 := rfl
> 
> @[simp] lemma deltaFlag_recSucc_right (b s n) :
>   deltaFlag (app s (recΔ b s n)) = 0 := by
>   simp [deltaFlag]
> 
> /-- Strict drop of `deltaFlag` for the recursion-successor rewrite. -/
> lemma flag_drop_recSucc (b s n : Trace) :
>   deltaFlag (app s (recΔ b s n)) < deltaFlag (recΔ b s (delta n)) := by
>   simp [deltaFlag]
> 
> /-- Canonical recursion-depth κ: max-based; +2 bump at `recΔ _ _ (delta _)`. -/
> def kappa : Trace → Nat
> | .void                => 0
> | .delta t             => kappa t
> | .integrate t         => kappa t
> | .merge a b           => Nat.max (kappa a) (kappa b)
> | .recΔ b s (.delta n) => Nat.max (Nat.max (kappa b) (kappa s)) (kappa n) + 2
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
>     Nat.max (Nat.max (kappa b) (kappa s)) (kappa n) + 2 := rfl
> 
> /-- κ strictly drops for every rec-succ step. -/
> lemma kappa_drop_recSucc (b s n : Trace) :
>   kappa (app s (recΔ b s n)) < kappa (recΔ b s (delta n)) := by
>   -- shared base term
>   let base : Nat := Nat.max (Nat.max (kappa b) (kappa s)) (kappa n)
> 
>   /- 1.  κ(recΔ … n) ≤ base + 1 (case-split on n) -/
>   have h_rec : kappa (recΔ b s n) ≤ base + 1 := by
>     cases n with
>     | delta _ => simp [kappa, base, Nat.add_comm, Nat.add_left_comm]
>     | _       => simp [kappa, base]
> 
>   /- 2. κ(merge s …) ≤ base + 1 via max_le -/
>   have h_merge : kappa (app s (recΔ b s n)) ≤ base + 1 := by
>     have h_s : kappa s ≤ base :=
>       (Nat.le_max_right _ _).trans (Nat.le_max_left _ _)
>     have : max (kappa s) (kappa (recΔ b s n)) ≤ base + 1 :=
>       max_le (Nat.le_trans h_s (Nat.le_succ _)) h_rec
>     simpa [kappa, kappa_merge] using this
> 
>   /- 3. RHS definitional value -/
>   have h_rhs : kappa (recΔ b s (delta n)) = base + 2 := by
>     simp [kappa_rec_delta, base]
> 
>   /- 4. Assemble strict inequality -/
>   have : kappa (app s (recΔ b s n)) < base + 2 :=
>     Nat.lt_of_le_of_lt h_merge (Nat.lt_succ_self (base + 1))
>   simpa [h_rhs] using this
> 
> /-- Combined lexicographic measure (κ, μ) using MetaSN.mu. -/
> noncomputable def innerMeasure (t : Trace) : Nat × Ordinal := (kappa t, MetaSN.mu t)
> 
> /-- Lexicographic order on the inner pair `(κ, μ)`. -/
> def LexNatOrd : (Nat × Ordinal) → (Nat × Ordinal) → Prop :=
>   Prod.Lex (· < ·) (· < ·)
> 
> /-- The inner lexicographic order is well-founded. -/
> lemma wf_LexNatOrd : WellFounded LexNatOrd := by
>   exact WellFounded.prod_lex Nat.lt_wfRel.wf Ordinal.lt_wf
> 
> /-- When κ drops strictly, we get lexicographic decrease inside the inner measure. -/
> lemma inner_drop_left {a b : Trace} (hk : kappa b < kappa a) :
>     LexNatOrd (innerMeasure b) (innerMeasure a) := by
>   unfold LexNatOrd innerMeasure
>   exact Prod.Lex.left _ _ hk
> 
> /-- When κ is equal and μ drops strictly, we get lexicographic decrease (inner). -/
> lemma inner_drop_right {a b : Trace} (hμ : MetaSN.mu b < MetaSN.mu a)
>     (hk : kappa b = kappa a) :
>     LexNatOrd (innerMeasure b) (innerMeasure a) := by
>   unfold LexNatOrd innerMeasure
>   rw [hk]
>   exact Prod.Lex.right _ hμ
> 
> 
> noncomputable def measure (t : Trace) : Nat × (Nat × Ordinal) :=
>   (deltaFlag t, innerMeasure t)
> 
> 
> def LexOrder : (Nat × (Nat × Ordinal)) → (Nat × (Nat × Ordinal)) → Prop :=
>   Prod.Lex (· < ·) LexNatOrd
> 
> /-- Well-foundedness of the outer lexicographic order. -/
> lemma wf_LexOrder : WellFounded LexOrder := by
>   exact WellFounded.prod_lex Nat.lt_wfRel.wf wf_LexNatOrd
> 
> /-- When `deltaFlag` drops strictly, we get a lexicographic decrease. -/
> lemma drop_flag {a b : Trace} (hδ : deltaFlag b < deltaFlag a) :
>     LexOrder (measure b) (measure a) := by
>   unfold LexOrder measure
>   exact Prod.Lex.left _ _ hδ
> 
> /-- Lift an inner `(κ, μ)` decrease to the full measure when `deltaFlag`
>     stays equal. -/
> lemma lift_inner {a b : Trace}
>     (hinner : LexNatOrd (innerMeasure b) (innerMeasure a))
>     (hδ : deltaFlag b = deltaFlag a) :
>     LexOrder (measure b) (measure a) := by
>   unfold LexOrder measure at *
>   cases hδ
>   exact Prod.Lex.right _ hinner
> 
> 
> -- keep diagnostics light to avoid noise during build
> set_option maxHeartbeats 800000
> set_option linter.unusedSimpArgs false
> set_option diagnostics true
> 
> -- set_option trace.Elab.process true
> /-- Every primitive step strictly decreases the lexicographic measure. -/
> theorem measure_decreases : ∀ {a b : Trace}, Step a b → LexOrder (measure b) (measure a)
> | _, _, Step.R_int_delta t => by
>     -- `deltaFlag` is `0` on both terms; rely on inner κ/μ logic.
>     by_cases hκ : kappa t = 0
>     ·
>       have hinner := inner_drop_right (MetaSN.mu_void_lt_integrate_delta t)
>         (by simpa [kappa_integrate, kappa_delta, kappa_void, hκ])
>       have hδ : deltaFlag void = deltaFlag (integrate (delta t)) := by
>         simp [deltaFlag]
>       exact lift_inner hinner hδ
>     ·
>       have hpos : 0 < kappa t := Nat.pos_of_ne_zero hκ
>       have hinner := inner_drop_left (by
>         simpa [kappa_integrate, kappa_delta, kappa_void] using hpos)
>       have hδ : deltaFlag void = deltaFlag (integrate (delta t)) := by
>         simp [deltaFlag]
>       exact lift_inner hinner hδ
> 
> | _, _, Step.R_merge_void_left t => by
>     have hinner := inner_drop_right (MetaSN.mu_lt_merge_void_left t)
>       (by simpa [kappa_merge, kappa_void, Nat.max_eq_right (Nat.zero_le _)])
>     have hδ : deltaFlag t = deltaFlag (merge void t) := by
>       simp [deltaFlag]
>     exact lift_inner hinner hδ.symm
> 
> | _, _, Step.R_merge_void_right t => by
>     have hinner := inner_drop_right (MetaSN.mu_lt_merge_void_right t)
>       (by simpa [kappa_merge, kappa_void, Nat.max_eq_left (Nat.zero_le _)])
>     have hδ : deltaFlag t = deltaFlag (merge t void) := by
>       simp [deltaFlag]
>     exact lift_inner hinner hδ.symm
> 
> | _, _, Step.R_merge_cancel t => by
>     have hinner := inner_drop_right (MetaSN.mu_lt_merge_cancel t)
>       (by simpa [kappa_merge, Nat.max_self])
>     have hδ : deltaFlag t = deltaFlag (merge t t) := by
>       simp [deltaFlag]
>     exact lift_inner hinner hδ.symm
> 
> | _, _, Step.R_rec_zero b s => by
>     have hb_le : kappa b ≤ kappa (recΔ b s void) := by
>       have h1 : kappa b ≤ Nat.max (kappa b) (kappa s) := Nat.le_max_left _ _
>       have h2 : Nat.max (kappa b) (kappa s) ≤
>                 Nat.max (Nat.max (kappa b) (kappa s)) (kappa void) := Nat.le_max_left _ _
>       simpa [kappa, kappa_void] using le_trans h1 h2
>     by_cases hb_eq : kappa b = kappa (recΔ b s void)
>     ·
>       have hinner := inner_drop_right (MetaSN.mu_lt_rec_base b s) hb_eq
>       have hδ : deltaFlag b = deltaFlag (recΔ b s void) := by simp [deltaFlag]
>       exact lift_inner hinner hδ
>     ·
>       have hinner := inner_drop_left (Nat.lt_of_le_of_ne hb_le hb_eq)
>       have hδ : deltaFlag b = deltaFlag (recΔ b s void) := by simp [deltaFlag]
>       exact lift_inner hinner hδ
> 
> | _, _, Step.R_rec_succ b s n => by
>     -- Here `deltaFlag` drops strictly, primary component handles it.
>     exact drop_flag (flag_drop_recSucc b s n)
> 
> | _, _, Step.R_eq_refl a => by
>     by_cases hκ : kappa a = 0
>     ·
>       have hinner := inner_drop_right (MetaSN.mu_void_lt_eq_refl a)
>         (by simpa [kappa_eqW, kappa_void, Nat.max_self, hκ])
>       have hδ : deltaFlag void = deltaFlag (eqW a a) := by simp [deltaFlag]
>       exact lift_inner hinner hδ
>     ·
>       have hpos : 0 < kappa a := Nat.pos_of_ne_zero hκ
>       have hinner := inner_drop_left (by
>         simpa [kappa_eqW, kappa_void, Nat.max_self] using hpos)
>       have hδ : deltaFlag void = deltaFlag (eqW a a) := by simp [deltaFlag]
>       exact lift_inner hinner hδ
> 
> | _, _, Step.R_eq_diff (a:=a) (b:=b) hneq => by
>     have hinner := inner_drop_right (MetaSN.mu_lt_eq_diff a b) (by
>       simpa [kappa_eqW, kappa_integrate, kappa_merge,
>              Nat.max_comm, Nat.max_assoc, Nat.max_left_comm])
>     have hδ : deltaFlag (integrate (merge a b)) = deltaFlag (eqW a b) := by
>       simp [deltaFlag]
>     exact lift_inner hinner hδ.symm
> 
> /-- The reverse step relation (for forward normalization). -/






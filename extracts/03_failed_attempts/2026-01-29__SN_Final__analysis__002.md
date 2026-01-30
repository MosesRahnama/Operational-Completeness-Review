Purpose: Evidence extract (failed_attempts/analysis) documenting a failure or relevance; Uses deltaFlag and ? +2 bump to force strict drop, while depending on external MetaSN ? lemmas.
Contents: Metadata header + excerpt from the source file.
Context: SN_Final.lean excerpt showing the deltaFlag primary bit and κ with +2 bump at recΔ (delta _), used in a lexicographic termination measure.
Source: MUST_Review/Legacy/Meta_Lean_Archive/SN_Final.lean
SHA256: 55B2AD5600522515D9AA51729D5628CE39504DF5DD36B235B773F3F045BFB180
FailureExplanation: Termination is forced by ad hoc measure tweaks (deltaFlag + κ +2) and still relies on external MetaSN μ-drop lemmas.
FailureModeTags: nondecreasing_measure; unsupported_inference

Excerpt:
> simp [deltaFlag]
>
> /-- Strict drop of `deltaFlag` for the recursion-successor rewrite. -/
> lemma flag_drop_recSucc (b s n : Trace) :
>   deltaFlag (merge s (recΔ b s n)) < deltaFlag (recΔ b s (delta n)) := by
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
>   kappa (merge s (recΔ b s n)) < kappa (recΔ b s (delta n)) := by
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
>   have h_merge : kappa (merge s (recΔ b s n)) ≤ base + 1 := by
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
>   have : kappa (merge s (recΔ b s n)) < base + 2 :=
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





Purpose: Evidence extract (failed_attempts/proof-attempt) documenting a failure or relevance; Duplicate of Legacy\\MetaMD_Archive\\Termination_Lex3.lean.
Contents: Metadata header + excerpt from the source file.
Context: Excerpt from Termination_Lex3 showing the μ3 definition and partial rule-drop lemmas.
Source: Legacy/MetaMD_Archive/Termination_Lex3.lean
SHA256: 48CC2C7FA7C666D9EFDA08D492EA88F49128BD160F2C8D258BA5384BB0A79408
FailureExplanation: The file only sketches a triple-lex measure with a few rule drops; it does not cover all rules or conclude SN.
FailureModeTags: nonconvergence

Excerpt:
> namespace MetaSN_Lex3
> 
> @[simp] def deltaFlag (_t : Trace) : Nat := 0
> 
> noncomputable def μ3 (t : Trace) : Nat × (Multiset ℕ × Ordinal) :=
>   (deltaFlag t, (MetaSN_DM.kappaM t, MetaSN.mu t))
> 
> @[simp] def Lex3 : (Nat × (Multiset ℕ × Ordinal)) → (Nat × (Multiset ℕ × Ordinal)) → Prop :=
>   Prod.Lex (· < ·) MetaSN_DM.LexDM
> 
> /- Well-foundedness: product of Nat.< and the KO7 inner LexDM -/
> lemma wf_Lex3 : WellFounded Lex3 := by
>   have wfN : WellFounded (fun a b : Nat => a < b) := Nat.lt_wfRel.wf
>   have wfInner : WellFounded MetaSN_DM.LexDM := MetaSN_DM.wf_LexDM
>   exact WellFounded.prod_lex wfN wfInner
> 
> /-- rec_zero decreases strictly in the triple-lex order (from LHS to RHS).
>     Orientation: we show μ3(b) < μ3(recΔ b s void), i.e. the measure drops after
>     reducing `recΔ b s void → b`. -/
> lemma lex3_drop_R_rec_zero (b s : Trace) :
>     Lex3 (μ3 b) (μ3 (recΔ b s void)) := by
>   classical
>   -- Outer: tie on deltaFlag (definally 0), so we can take the right branch.
>   -- Inner: κᴹ drops by DM, so take the left branch of inner lex.
>   dsimp [μ3, Lex3, deltaFlag]
>   apply Prod.Lex.right
>   -- Now the goal is MetaSN_DM.LexDM on the inner pair.
>   -- Build it by the left constructor from the DM drop on κᴹ.
>   apply Prod.Lex.left
>   exact MetaSN_DM.dm_drop_R_rec_zero b s
> 
> /-- merge void-left: `merge void t → t` drops in Lex3. -/
> lemma lex3_drop_R_merge_void_left (t : Trace) :
>     Lex3 (μ3 t) (μ3 (merge void t)) := by
>   classical
>   -- rewrite κ equality to establish tie, then use μ-drop
>   simp [μ3, Lex3, deltaFlag]
>   apply Prod.Lex.right
>   apply Prod.Lex.right
>   simpa using MetaSN.mu_lt_merge_void_left t
> 
> /-- merge void-right: `merge t void → t` drops in Lex3. -/
> lemma lex3_drop_R_merge_void_right (t : Trace) :
>     Lex3 (μ3 t) (μ3 (merge t void)) := by
>   classical
>   -- rewrite κ equality to establish tie, then use μ-drop
>   simp [μ3, Lex3, deltaFlag]
>   apply Prod.Lex.right
>   apply Prod.Lex.right
>   simpa using MetaSN.mu_lt_merge_void_right t





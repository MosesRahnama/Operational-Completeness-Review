Purpose: Evidence extract (failed_attempts/analysis) documenting a failure or relevance; Duplicate of MUST_Review\Legacy\Meta_Lean_Archive\MuLexSN.lean.
Contents: Metadata header + excerpt from the source file.
Context: MuLexSN.lean excerpt showing the kappaTop bit and the claimed lexicographic decrease/strong normalization.
Source: MUST_Review/Legacy/Meta_Lean_Archive/MuLexSN.lean
SHA256: C5DC89EC2EAE442F0DFFA77C53CB4D3C5038A1A9C0D581669F0A05C2D6EE21F3
FailureExplanation: The proof still depends on MetaSN μ decrease lemmas from legacy termination, so the key measure is imported rather than constructed or validated here.
FailureModeTags: unsupported_inference

Excerpt:
> def kappaTop : Trace → Nat
> | recΔ _ _ _ => 1
> | _ => 0
>
> @[simp] lemma kappaTop_rec (b s n : Trace) : kappaTop (recΔ b s n) = 1 := rfl
> @[simp] lemma kappaTop_void : kappaTop void = 0 := rfl
> @[simp] lemma kappaTop_delta (t) : kappaTop (delta t) = 0 := rfl
> @[simp] lemma kappaTop_integrate (t) : kappaTop (integrate t) = 0 := rfl
> @[simp] lemma kappaTop_merge (a b) : kappaTop (merge a b) = 0 := rfl
> @[simp] lemma kappaTop_eqW (a b) : kappaTop (eqW a b) = 0 := rfl
Call Jayaram Select
> noncomputable def μκ (t : Trace) : Nat × Ordinal := (kappaTop t, MetaSN.mu t)
>
> def LexNatOrdTop : (Nat × Ordinal) → (Nat × Ordinal) → Prop :=
>   Prod.Lex (· < ·) (· < ·)
>
> lemma wf_LexNatOrdTop : WellFounded LexNatOrdTop := by
>   refine WellFounded.prod_lex ?_ ?_
>   · exact Nat.lt_wfRel.wf
>   · intro _; exact Ordinal.lt_wf
>
> lemma lift_mu_drop {t u : Trace}
>   (hμ : MetaSN.mu u < MetaSN.mu t) (hκ : kappaTop u = kappaTop t) :
>   LexNatOrdTop (μκ u) (μκ t) := by
>   unfold LexNatOrdTop μκ; simpa [hκ] using Prod.Lex.right _ hμ
>
> lemma drop_rec_succ (b s n : Trace) :
>   LexNatOrdTop (μκ (merge s (recΔ b s n))) (μκ (recΔ b s (delta n))) := by
>   unfold LexNatOrdTop μκ; apply Prod.Lex.left; simp [kappaTop]
>
> def StepRev : Trace → Trace → Prop := fun a b => Step b a
>
> lemma μκ_decreases : ∀ {a b}, Step a b → LexNatOrdTop (μκ b) (μκ a)
> | _, _, Step.R_int_delta t =>
>   lift_mu_drop (MetaSN.mu_void_lt_integrate_delta t) (by simp [kappaTop])
> | _, _, Step.R_merge_void_left t =>
>   lift_mu_drop (MetaSN.mu_lt_merge_void_left t) (by simp [kappaTop])
> | _, _, Step.R_merge_void_right t =>
>   lift_mu_drop (MetaSN.mu_lt_merge_void_right t) (by simp [kappaTop])
> | _, _, Step.R_merge_cancel t =>
>   lift_mu_drop (MetaSN.mu_lt_merge_cancel t) (by simp [kappaTop])
> | _, _, Step.R_rec_zero b s =>
>   lift_mu_drop (MetaSN.mu_lt_rec_base b s) (by simp [kappaTop])
> | _, _, Step.R_rec_succ b s n =>
>     drop_rec_succ b s n
> | _, _, Step.R_eq_refl a =>
>   lift_mu_drop (MetaSN.mu_void_lt_eq_refl a) (by simp [kappaTop])
> | _, _, Step.R_eq_diff hneq =>
>   lift_mu_drop (MetaSN.mu_lt_eq_diff _ _) (by simp [kappaTop])
>
> /-- Strong normalization via lexicographic (kappaTop, μ). -/
> theorem strong_normalization : WellFounded StepRev := by
>   have wfLex := wf_LexNatOrdTop
>   have wfInv : WellFounded (InvImage LexNatOrdTop μκ) :=
>     InvImage.wf _ wfLex
>   have sub : Subrelation StepRev (InvImage LexNatOrdTop μκ) := by
>     intro a b h; exact μκ_decreases h
>   exact Subrelation.wf sub wfInv
>
> end OperatorKernelO6.MetaLex





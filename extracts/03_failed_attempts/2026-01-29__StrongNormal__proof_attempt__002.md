Context: Excerpt showing the final SN theorem relying on μ lemmas and a measure-drop lemma defined elsewhere.
Source: C:\Users\Moses\OpComp\MUST_Review\MetaMD_Archive\StrongNormal.md
SHA256: 6190AC5058EBD2772FCF1F6F826E39993969FBEB8A05A3B604CA1695F008991F
FailureExplanation: The proof depends on μ lemmas (e.g., mu_lt_eq_diff) and a measure_drop_of_step lemma that are disputed or incomplete in other files, so SN is not established here.
FailureModeTags: proof_obligation_stuck

Excerpt:
> -- eqW a b → integrate (merge a b)  (κ equal; μ drops)
> have hμ := mu_lt_eq_diff a b
> have hk : kappaD (.eqW a b) = kappaD (.integrate (.merge a b)) := by
>   simp [kappaD, Nat.max_assoc, Nat.max_comm, Nat.max_left_comm]
> exact drop_right hμ hk
> 
> /-- Final SN: no infinite forward reductions. -/
> noncomputable theorem strong_normalization_final : WellFounded StepRev := by
>   have wfMeasure : WellFounded (InvImage LexNatOrd muHat) :=
>     InvImage.wf _ wf_LexNatOrd
>   have sub : Subrelation StepRev (InvImage LexNatOrd muHat) := by
>     intro a b h; exact measure_drop_of_step h
>   exact Subrelation.wf sub wfMeasure

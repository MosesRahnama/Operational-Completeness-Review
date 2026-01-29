Context: Excerpt showing lexicographic decrease proof using kappaTop and μ without addressing kappaTop increases on some rules.
Source: C:\Users\Moses\OpComp\MUST_Review\MetaMD_Archive\Termination_Lex.md
SHA256: D5E107C8769B50BEF7F37839AC14B09FCE092F65C9B6B1EA148905D2E89CB9D6
FailureExplanation: The lex proof applies a left-branch kappaTop decrease broadly, but kappaTop can increase for some non-rec_succ transitions, so the measure drop is not justified.
FailureModeTags: nondecreasing_measure; unsupported_inference

Excerpt:
> | R_rec_succ b s n =>
>     -- left strictly drops: 0 < 1
>     unfold LexNatOrdTop μκTop
>     apply Prod.Lex.left
>     simp [kappaTop]
> 
> /- Strong normalization via the lexicographic measure -------------------------/
> 
> theorem strong_normalization_lex : WellFounded (fun a b => Step b a) := by
>     have wf_lex : WellFounded LexNatOrdTop := wf_LexNatOrdTop
>     refine Subrelation.wf ?_ (InvImage.wf (f := μκTop) wf_lex)
>     intro x y hxy; simpa using mu_kappa_decreases_lex hxy

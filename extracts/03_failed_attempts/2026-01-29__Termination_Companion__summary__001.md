Context: File review extract.
Source: C:\Users\Moses\OpComp\MUST_Review\Termination_Companion.md
SHA256: CE216997787C2E1DCC6FF046DD6416463CD7E457FC286CE273DFD72FFCD6B435
FailureExplanation: Analysis or notes documenting failures, blockers, or unproven claims.
FailureModeTags: constraint_violation; inconsistency

Excerpt:
> # MetaSN Strong-Normalisation Proof  Full Sketch, Audit Notes, and the *rec_succ_bound* Issue
> ## 1. File Layout ( 1 200 LOC)
> | File | Purpose | Size |
> |------|---------|------|
> | `Termination.lean` | ordinal toolbox, core -lemmas, kernel rules, `` measure, SN proof | ~1250 LOC |
> The file lives in namespace `MetaSN`; it imports the operator kernel plus **Mathlib**'s ordinal theory.
> ---
> ## 2. The Measure `` and the Eight Decrease Cases
> ```lean
>  : Trace  Ordinal

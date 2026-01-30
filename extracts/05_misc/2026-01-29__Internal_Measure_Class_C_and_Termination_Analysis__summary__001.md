Purpose: Evidence extract (misc/reference) documenting a failure or relevance; Background research memo; not a proof artifact.
Contents: Metadata header + excerpt from the source file.
Context: Background research memo; not a proof artifact.
Source: MUST_Review/important_2/Research/Internal_Measure_Class_C(Σ)_and_Termination_Analysis.md
SHA256: 6B6AB67717B327FAB5F67605D589F2D5F800E03AC32E3BD9E697C78EA9C1F781
FailureExplanation: Background research memo; not a proof artifact.
FailureModeTags: 

Excerpt:
> # Internal Measure Class C(Σ) and Termination Analysis
> *Converted from: Internal Measure Class C(Σ) and Termination Analysis.docx*
> ---
> # Internal Measure Class C(Σ) and Termination Analysis
> ## C(Σ): Operator-Definable Measure in KO7
> Definition: In the KO7 system (OperatorKernelO6), every rewrite rule is oriented by an ordinal measure μ: Trace → Ordinal. The measure μ is defined recursively on the trace signature Σ = {void, delta, integrate, merge, recΔ, eqW} as follows[1][2]:
> - Base: μ(void) = 0.
> - Delta: μ(delta t) = ω<sup>5</sup> · (μ(t) + 1) + 1.
> - Integrate: μ(integrate t) = ω<sup>4</sup> · (μ(t) + 1) + 1.
> - Merge: μ(merge a b) = ω<sup>3</sup> · (μ(a) + 1) + ω<sup>2</sup> · (μ(b) + 1) + 1.
> - Recursion: μ(recΔ b s n) = ω<sup>(μ(n) + μ(s) + 6)</sup> + ω · (μ(b) + 1) + 1.
> - Equality: μ(eqW a b) = ω<sup>(μ(a) + μ(b) + 9)</sup> + 1.





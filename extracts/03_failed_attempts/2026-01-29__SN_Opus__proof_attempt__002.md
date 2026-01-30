Purpose: Evidence extract (failed_attempts/proof-attempt) documenting a failure or relevance; The proof sketch uses linarith and informal ordinal inequalities to assert ?? decreases, which is not justified under project constraints.
Contents: Metadata header + excerpt from the source file.
Context: Excerpt showing μ₂ decrease proofs for rec_zero and rec_succ relying on simp/linarith in ordinal arithmetic.
Source: MUST_Review/MetaMD_Archive/SN_Opus.md
SHA256: 4F57F01CB5D3BA9494E776B321F66CE4D422C561BDDC8781F78316E3DD031003
FailureExplanation: The proof sketch uses linarith and informal ordinal inequalities to assert μ₂ decreases, which is not justified under project constraints.
FailureModeTags: constraint_violation; unsupported_inference

Excerpt:
> -- R_rec_zero: recΔ b s void → b
> theorem mu2_decrease_rec_zero (b s : Trace) :
>     μ₂ b < μ₂ (recΔ b s void) := by
>   unfold μ₂
>   apply double_exp_monotone
>   simp [baseLayer]
>   linarith
> 
> -- R_rec_succ: recΔ b s (delta n) → merge s (recΔ b s n)
> -- This is the crucial case that motivated the double-exponent construction
> theorem mu2_decrease_rec_succ (b s n : Trace) :
>     μ₂ (merge s (recΔ b s n)) < μ₂ (recΔ b s (delta n)) := by
>   unfold μ₂
>   apply double_exp_monotone
>   simp [baseLayer]
>   -- Key insight: delta wrapper creates exponential gap





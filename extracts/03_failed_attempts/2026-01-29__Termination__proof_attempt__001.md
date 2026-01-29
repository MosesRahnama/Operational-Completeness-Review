Context: Excerpt from Termination showing the μκ_decreases lemma with an explicit admit in the rec_zero case.
Source: C:\Users\Moses\OpComp\MUST_Review\Legacy\MetaMD_Archive\Termination.md
SHA256: E17BABC9A0B8F87C25D706492541DFFDE6ABEC91E5BDBE8F921C258FFE62D246
FailureExplanation: The termination proof includes an `admit` in μκ_decreases (rec_zero) and uses tactic-heavy imports, leaving SN unproven under constraints.
FailureModeTags: proof_obligation_stuck; constraint_violation

Excerpt:
> lemma μκ_decreases {a b : Trace} (h : OperatorKernelO6.Step a b) :
>     LexNatOrd (μκ b) (μκ a) := by
>   cases h with
>   | R_int_delta t =>
>       exact μκ_drop_R_int_delta t
>   | R_merge_void_left t =>
>       exact μ_to_μκ (mu_lt_merge_void_left t) rfl
>   | R_merge_void_right t =>
>       exact μ_to_μκ (mu_lt_merge_void_right t) rfl
>   | R_merge_cancel t =>
>       exact μ_to_μκ (mu_lt_merge_cancel t) rfl
>   | R_rec_zero b s =>
>       -- κ may increase here; leave as pending hard wall.
>       admit
>   | R_rec_succ b s n => exact μκ_lex_drop_recSucc b s n
>   | R_eq_refl a => exact μκ_drop_R_eq_refl a
>   | R_eq_diff a b hneq =>
>       exact μκ_drop_R_eq_diff a b hneq
> 
> 
> 
> /-- Strong normalization of the kernel: there is no infinite `Step` chain.  We orient the relation
>     in the deleting direction and use the `(κ, μ)` lexicographic measure. -/

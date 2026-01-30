Purpose: Evidence extract (failed_attempts/proof-attempt) documenting a failure or relevance; The kappaTop proof applies a left-branch decrease in cases where kappaTop increases (e.g., merge void with rec?), so the lex decrease is invalid.
Contents: Metadata header + excerpt from the source file.
Context: Excerpt from Termination_Lex showing the kappaTop lex proof and the recΔ case in merge-void using the wrong lex direction.
Source: Legacy/MetaMD_Archive/Termination_Lex.md
SHA256: E370E4767AFAF6C5D15A2E3D8C8F225B08C9D8C3D8E4C9A5C02C0E96C14972A8
FailureExplanation: The kappaTop proof applies a left-branch decrease in cases where kappaTop increases (e.g., merge void with recΔ), so the lex decrease is invalid.
FailureModeTags: nondecreasing_measure; unsupported_inference

Excerpt:
> theorem mu_kappa_decreases_lex :
>   ∀ {a b : Trace}, Step a b → LexNatOrdTop (μκTop b) (μκTop a) := by
>   intro a b h
>   cases h with
>   | @R_int_delta t =>
>       have h_mu : mu void < mu (integrate (delta t)) := mu_void_lt_integrate_delta t
>       have h_k : kappaTop void = kappaTop (integrate (delta t)) := by simp
>       exact μ_to_μκ_top h_mu h_k
>   | R_merge_void_left =>
>     have h_mu : mu b < mu (merge void b) := mu_lt_merge_void_left b
>     cases b with
>     | void =>
>       simpa [kappaTop] using μ_to_μκ_top h_mu (by simp [kappaTop])
>     | delta t =>
>       simpa [kappaTop] using μ_to_μκ_top h_mu (by simp [kappaTop])
>     | integrate t =>
>       simpa [kappaTop] using μ_to_μκ_top h_mu (by simp [kappaTop])
>     | merge a c =>
>       simpa [kappaTop] using μ_to_μκ_top h_mu (by simp [kappaTop])
>     | recΔ b' s n =>
>       unfold LexNatOrdTop μκTop
>       apply Prod.Lex.left
>       simp [kappaTop]
>     | eqW a c =>
>       simpa [kappaTop] using μ_to_μκ_top h_mu (by simp [kappaTop])
>   | R_merge_void_right =>
>     have h_mu : mu b < mu (merge b void) := mu_lt_merge_void_right b
>     cases b with
>     | void =>
>       simpa [kappaTop] using μ_to_μκ_top h_mu (by simp [kappaTop])
>     | delta t =>
>       simpa [kappaTop] using μ_to_μκ_top h_mu (by simp [kappaTop])
>     | integrate t =>
>       simpa [kappaTop] using μ_to_μκ_top h_mu (by simp [kappaTop])
>     | merge a c =>
>       simpa [kappaTop] using μ_to_μκ_top h_mu (by simp [kappaTop])
>     | recΔ b' s n =>
>       unfold LexNatOrdTop μκTop
>       apply Prod.Lex.left
>       simp [kappaTop]
>     | eqW a c =>
>       simpa [kappaTop] using μ_to_μκ_top h_mu (by simp [kappaTop])
>   | R_merge_cancel =>
>     have h_mu : mu b < mu (merge b b) := mu_lt_merge_cancel b
>     cases b with
>     | void =>
>       simpa [kappaTop] using μ_to_μκ_top h_mu (by simp [kappaTop])
>     | delta t =>
>       simpa [kappaTop] using μ_to_μκ_top h_mu (by simp [kappaTop])
>     | integrate t =>
>       simpa [kappaTop] using μ_to_μκ_top h_mu (by simp [kappaTop])
>     | merge a c =>
>       simpa [kappaTop] using μ_to_μκ_top h_mu (by simp [kappaTop])
>     | recΔ b' s n =>
>       unfold LexNatOrdTop μκTop
>       apply Prod.Lex.left
>       simp [kappaTop]
>     | eqW a c =>
>       simpa [kappaTop] using μ_to_μκ_top h_mu (by simp [kappaTop])
>   | @R_rec_zero b s =>
>       have hμ : mu b < mu (recΔ b s void) := mu_lt_rec_base b s
>       cases hb : b with
>       | recΔ b' s' n' =>
>           -- equal kappaTop (both 1), rely on μ decrease
>           have : kappaTop (recΔ b' s' n') = kappaTop (recΔ b s void) := by simp
>           -- target is b (t') source is recΔ b s void (t)
>           -- we need Lex μκTop b μκTop (recΔ b s void)
>           -- so pass hμ and equality
>           simpa [hb] using (μ_to_μκ_top (t' := b) (t := recΔ b s void) hμ (by simp [hb]))
>       | void
>       | delta _
>       | integrate _
>       | merge _ _
>       | eqW _ _ =>
>           -- kappaTop drops 1 -> 0, left decrease
>           unfold LexNatOrdTop μκTop
>           apply Prod.Lex.left
>           simp [kappaTop, hb]





Purpose: Evidence extract (failed_attempts/analysis) documenting a failure or relevance; Duplicate of FRV2-0159.
Contents: Metadata header + excerpt from the source file.
Context: Planning memo synthesizing AI suggestions for strong normalization, identifying which branches fail and which lemmas are missing.
Source: Legacy/MetaMD_Archive/AI_BrainStorm_SN.md
SHA256: 141EF8AC9DDBE7217FDAD41236DCD26CD24958845EBF924E8C12746DD1668FDD
FailureExplanation: Documents repeated repair planning around missing μ lemmas and brittle κ handling, illustrating nonconvergence rather than a completed proof.
FailureModeTags: nonconvergence; environment_mismatch; proof_obligation_stuck

Excerpt:
> • The lexicographic measure μ̂(t) := (κ(t), μ(t)) under Prod.Lex ... is the right scaffold.
> • κ must bump exactly once: +1 only at recΔ _ _ (delta _).
> • The only lemma that sometimes “goes missing” in builds is mu_lt_eq_diff; it lives in Meta.Termination and must stay visible in MuCore.
>
> • SN_Final and the two “Claude” files keep κ equal in the rec_succ δ-case; that forces a μ-drop that depends on the impossible domination inequality.
>
> Step 1 Fix MuCore ... Rewrite termA_le / termB_le ... Re-enable (or simply export) mu_lt_eq_diff ...





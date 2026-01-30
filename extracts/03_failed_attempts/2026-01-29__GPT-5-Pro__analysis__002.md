Purpose: Evidence extract (failed_attempts/analysis) documenting a failure or relevance; Duplicate of FRV2-0181.
Contents: Metadata header + excerpt from the source file.
Context: Lists disallowed lemmas and invalid proof shortcuts used in prior attempts.
Source: MUST_Review/chats/GPT-5-Pro.md
SHA256: 2CC1DCF2833D2178BC911132E0BF5B45241295BE2C3CC52BB9F67BDF2C68605D
FailureExplanation: Lists disallowed lemmas and invalid proof shortcuts used in prior attempts.
FailureModeTags: unsupported_inference; invalid_rewrite; protocol_violation

Excerpt:
> alright — deep breath — here’s what went wrong, what we’re not going to try again, and the clean fix that gets you to green using the lexicographic measure (κ, μ) with κ strictly dropping only on recΔ‑succ and μ dropping on the other 7 rules. Kernel stays exactly as in your repo (incl. R_eq_diff with a ≠ premise). No bogus ω‑power lemmas, no hidden axioms, no sorrys.
> Below I give:
> a 1‑page failed‑attempts audit (so we don’t loop back),
> the per‑rule math↔tactic mapping,
> a tiny dependency scan, and
> the exact code patches (two files): a new μ‑drop for eqW_diff in MuCore.lean and the final single SN harness in SN_Final.lean using Prod.Lex + InvImage.wf.





Context: File review extract.
Source: C:\Users\Moses\OpComp\MUST_Review\copilot-instructions copy.md
SHA256: 36F1E4B2263E492F335301243DF6333EA6224A7C46890AD9F9403015ADD78200
FailureExplanation: Analysis or notes documenting failures, blockers, or unproven claims.
FailureModeTags: constraint_violation; proof_obligation_stuck; nonconvergence

Excerpt:
> ---
> applyTo: "**"
> ---
> # STRICT EXECUTION CONTRACT
> Read this first. Obey it exactly. If you cannot, say so.
> A) Branch-by-branch rfl gate
> - For any claim about a pattern-matched function f: enumerate all defining clauses.
> - For each clause, attempt rfl (definitional equality). Record pass/fail.
> - If any clause fails: name the failing pattern; give the corrected per-branch statement; do not assert a single global equation for f.
> - Provide a minimal counterexample when a global law fails on some branch.

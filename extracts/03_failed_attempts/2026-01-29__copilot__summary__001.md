Purpose: Evidence extract (failed_attempts/analysis) documenting a failure or relevance; Analysis or notes documenting failures, blockers, or unproven claims.
Contents: Metadata header + excerpt from the source file.
Context: File review extract.
Source: MUST_Review/copilot.md
SHA256: 0A3C0CDE0DBF4CBFBF09C912DE60CC2D2506FD6742E7C282648707B38E66AA5A
FailureExplanation: Analysis or notes documenting failures, blockers, or unproven claims.
FailureModeTags: constraint_violation; nonconvergence

Excerpt:
> ## 0. TL;DR
> 1. **Kernel is sacred.** 7 constructors, 8 rules. No edits unless explicitly approved.
> 2. **Inside kernel:** no `Nat`, `Bool`, numerals, `simp`, `rfl`, patternmatches on nonkernel stuff. Only `Prop` + recursors.
> 3. **Meta land:** You may use Nat/Bool, classical, tactics, WF recursion, and mostly the imports/lemmas listed in 8.
> 4. **Main jobs:** SN, normalizejoin confluence, arithmetic via `rec`, internal equality via `eqW`, provability & Gdel.
> 5. **Allowed outputs:** `PLAN`, `CODE`, `SEARCH`, **CONSTRAINT BLOCKER** (formats in 6).
> 6. **Never drop, rename, or "simplify" rules or imports without approval.**
> ---
> ## 1. Project
> **Repo:** OperatorKernelO6 / OperatorMath





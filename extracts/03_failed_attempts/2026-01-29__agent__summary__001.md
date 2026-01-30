Purpose: Evidence extract (failed_attempts/analysis) documenting a failure or relevance; Analysis or notes documenting failures, blockers, or unproven claims.
Contents: Metadata header + excerpt from the source file.
Context: File review extract.
Source: MUST_Review/agent.md
SHA256: E519E998ECEE9D31A47ABBFBA3F95ADB55ADBA063E933A14E867F7266FF00C6C
FailureExplanation: Analysis or notes documenting failures, blockers, or unproven claims.
FailureModeTags: constraint_violation; nonconvergence

Excerpt:
> # AGENT.md  AllinOne AI Guide for OperatorKernelO6 / OperatorMath
> > **Audience:** LLMs/agents working on this repo.
> > **Prime Directive:** Dont touch the kernel. Dont hallucinate lemmas/imports. Dont add axioms.
> > **If unsure:** raise a **CONSTRAINT BLOCKER**.
> ---
> ## 0. TL;DR
> 1. **Kernel is sacred.** 6 constructors, 8 rules. No edits unless explicitly approved.
> 2. **Inside kernel:** no `Nat`, `Bool`, numerals, `simp`, `rfl`, patternmatches on nonkernel stuff. Only `Prop` + recursors.
> 3. **Meta land:** You may use Nat/Bool, classical, tactics, WF recursion, and mostly the imports/lemmas listed in 8.
> 4. **Main jobs:** SN, normalizejoin confluence, arithmetic via `rec`, internal equality via `eqW`, provability & Gdel.





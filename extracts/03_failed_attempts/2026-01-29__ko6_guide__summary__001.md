Context: File review extract.
Source: C:\Users\Moses\OpComp\MUST_Review\ko6_guide.md
SHA256: 55A6C1364B1DAE616E723CFF0AD8CBE1BAEE6AB7769A704D62CBCBCBA9CF0A0A
FailureExplanation: Analysis or notes documenting failures, blockers, or unproven claims.
FailureModeTags: constraint_violation; nonconvergence

Excerpt:
> # AGENT.md  AllinOne AI Guide for OperatorKernelO6 / OperatorMath
> > **Audience:** LLMs/agents working on this repo.
> > **Prime Directive:** Don't touch the kernel. Don't hallucinate lemmas/imports. Don't add axioms.
> > **If unsure:** raise a **CONSTRAINT BLOCKER**.
> ---
> ## 0. TL;DR
> 1. **Kernel is sacred.** 6 constructors, 8 rules. No edits unless explicitly approved.
> 2. **Inside kernel:** no `Nat`, `Bool`, numerals, `simp`, `rfl`, patternmatches on nonkernel stuff. Only `Prop` + recursors.
> 3. **Meta land:** You may use Nat/Bool, classical, tactics, WF recursion, and mostly the imports/lemmas listed in 8.
> 4. **Main jobs:** SN, normalizejoin confluence, arithmetic via `rec`, internal equality via `eqW`, provability & Gdel.

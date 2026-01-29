Context: Prescriptive fixes not validated here.
Source: C:\Users\Moses\OpComp\MUST_Review\chats\for_claude.md
SHA256: E584CED948532E69E8DCF2CB19936CFE50F1F4CA73B0B4BE28CDF9FE77100B14
FailureExplanation: Prescriptive fixes not validated here.
FailureModeTags: unsupported_inference

Excerpt:
> Got it. Here is an ultra-explicit “do-not-deviate” script to make Claude_SN (ClaudeSN in OperatorKernelO6/Meta/Measure.lean) green, with tiny, scoped edits only. It fixes exactly the errors you pasted and stays in its lane (structural κ + μ-lex). I’ll also include diagnostics settings and how to read Lean’s feedback so Sonnet can’t drift.
> What to fix, why, and how
> - Error 1: “Missing cases” in measure_decreases
> - Cause: You didn’t cover all Step constructors.
> - Fix: Add four missing branches with exactly the code below (R_rec_zero, R_rec_succ, R_eq_refl, R_eq_diff).
> - Error 2: “unknown identifier 't'” inside R_rec_zero

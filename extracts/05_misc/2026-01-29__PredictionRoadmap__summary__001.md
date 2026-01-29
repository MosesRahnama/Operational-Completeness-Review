Context: Planning roadmap; not a proof artifact.
Source: C:\Users\Moses\OpComp\MUST_Review\important_2\PredictionRoadmap.md
SHA256: A09CA7BE586F398B22B22983C271EA7CA8743525BD388B1F2471E2E54865F48C
FailureExplanation: Planning roadmap; not a proof artifact.
FailureModeTags: 

Excerpt:
> # Prediction Roadmap — What To Expect From Step 1 (Based on Past Behavior)
> Purpose: provide a step-by-step expectation map of typical failures and guardrails, derived from wrong-prediction patterns and review notes.
> ## 0. Immediate Checks (before any proof attempt)
> - P1 Branch realism: enumerate clauses, test rfl per-branch; do not assert global equalities over pattern-matched functions.
> - P2 Duplication stress: if a rule duplicates S, first show additive non-drop; only then use DM/MPO with premise “each RHS piece < removed LHS redex”.
> - P3 Symbol realism: NameGate/TypeGate; verify lemma names and arities exist in the current environment before use.
> References: Review/copilot-instructions.md (§SYSTEMIC FAILURE MODES), Meta/ContractProbes.lean.
> ## 1. Early-stage patterns to expect
> - rec_succ domination bound proposals (ω^5 payload vs variable towers) will fail globally. Expect a constraint or a parameterized lemma.
> - κ equality at rec_succ is often (wrongly) claimed. Expect strict κ drop or δ-flag handling instead.
> - Right-add transport of < on ordinals will resurface. Expect hazards; allow only guarded uses (le_add_of_nonneg_*), never global strict transport.
> - Additive measures under duplication will “almost work” but not strictly drop. Expect a pivot to DM (κᴹ) or a fallback μ/τ drop under ties.

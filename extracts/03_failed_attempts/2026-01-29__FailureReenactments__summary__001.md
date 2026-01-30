Purpose: Evidence extract (failed_attempts/analysis) documenting a failure or relevance; Duplicate of FRV2-0354.
Contents: Metadata header + excerpt from the source file.
Context: Failure catalog or commentary documenting incorrect assumptions or blockers.
Source: MUST_Review/important_2/FailureReenactments.md
SHA256: 5E298D6D52AF4D395B4C78CEE692D7599D6560A2F924204687EDC62359714497
FailureExplanation: Failure catalog or commentary documenting incorrect assumptions or blockers.
FailureModeTags: unsupported_inference

Excerpt:
> # Failure Reenactments — Step-by-Step (STRICT EXECUTION CONTRACT)
> Purpose: simulate, in minimal steps, how certain proof attempts failed and how we corrected them.
> Each reenactment links to WrongPredictions (RP-*) and simulations.
> ## RE-1: Global rec_succ domination (RP-1)
> - Step 1 (naive): Propose a global bound that the recursor tail is dominated by a fixed ω^5 payload.
> - Step 2 (failure): Parameter values (large μ s) defeat any fixed ω^5; global inequality not provable.
> - Step 3 (fix): Use δ-flag primary (KO7) so rec_succ is handled by 1→0 drop; no global μ bound needed.
> - Pointers: WrongPredictions RP-1; KO7 measure in Meta/Termination_KO7.lean; Simulations §5 (deltaFlag counterexample under merge-void shows branch-split discipline).
> ## RE-2: κ equality at rec_succ (RP-2)
> - Step 1 (naive): Claim κ(app s (recΔ b s n)) = κ(recΔ b s (δ n)).
> - Step 2 (failure): Branch rfl check reveals rec_succ increases κ on the RHS; equality fails.
> - Step 3 (fix): Either κ strictly drops (classic lex) or δ handles rec_succ (KO7).






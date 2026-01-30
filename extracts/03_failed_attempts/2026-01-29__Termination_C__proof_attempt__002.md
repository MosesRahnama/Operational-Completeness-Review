Purpose: Evidence extract (failed_attempts/proof-attempt) documenting a failure or relevance; Termination_C includes a dead-end section with commented sorries in kappaTop case analysis and relies on heavy automation, so the proof is incomplete.
Contents: Metadata header + excerpt from the source file.
Context: Excerpt showing commented sorries in the kappaTop case split for merge cases.
Source: MUST_Review/MetaMD_Archive/Termination_C.md
SHA256: A2883791A81B902CBE393B889FB8CCCAD2CB232BFD0B335979B220C625396204
FailureExplanation: Termination_C includes a dead-end section with commented sorries in kappaTop case analysis and relies on heavy automation, so the proof is incomplete.
FailureModeTags: constraint_violation; proof_obligation_stuck; unsupported_inference

Excerpt:
> --         cases b with
> --         | void => rfl
> --         | delta _ => rfl
> --         | integrate _ => rfl
> --         | merge _ _ => rfl
> --         | recΔ _ _ _ =>
> --           -- This case is impossible with our kappaTop assignment
> --           sorry
> --         | eqW _ _ =>
> --           -- This case is impossible with our kappaTop assignment
> --           sorry





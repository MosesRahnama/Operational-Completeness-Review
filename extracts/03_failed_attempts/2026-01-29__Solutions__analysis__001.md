Purpose: Evidence extract (failed_attempts/analysis) documenting a failure or relevance; Duplicate of FRV2-0423.
Contents: Metadata header + excerpt from the source file.
Context: Converted solutions digest with candidate outcomes and blockers.
Source: MUST_Review/markdown_output/Solutions.md
SHA256: E0D9309F765ED2B472EAA022E122CDCB484792575E240318941339D1B62FEC9C
FailureExplanation: Solution digest records non-monotone measures and CI violations (Bool/DecidableEq/Nat).
FailureModeTags: nondecreasing_measure; constraint_violation; proof_obligation_stuck

Excerpt:
> muPi: body may re-grow term, cannot supply monotone measure => SN doubtful; Bool|DecidableEq|Nat (spaces) fails CI.





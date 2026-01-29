# Failure Mode Legend

Primary modes (scientific categories)
- unsupported_inference: proof step not derivable in the system (fabricated lemma, unjustified assumption).
- inconsistency: mutually incompatible claims within the same reasoning trace.
- constraint_violation: uses forbidden axioms, external arithmetic, encodings, or tools.
- nonconvergence: repeated patching or proof search that never discharges obligations.

Mechanism tags (optional, technical)
- case_split_omission: assumes global equality despite definitional case splits.
- nondecreasing_measure: termination measure fails to strictly decrease (often under duplication).
- invalid_rewrite: applies algebraic rewrite not valid for the structure (e.g., ordinal add).
- undefined_symbol: references missing lemma/identifier.
- type_mismatch: ill-typed application or arity error.
- proof_obligation_stuck: explicit unsolved goals or Lean diagnostics block the proof.
- environment_mismatch: relies on absent imports or wrong namespace.
- protocol_violation: violates documented proof protocol (NameGate/TypeGate/Contract).

Use multiple tags if needed, separated by semicolons.


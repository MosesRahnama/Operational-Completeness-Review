Context: File review extract.
Source: C:\Users\Moses\OpComp\MUST_Review\AGENT copy.md
SHA256: BE9DED24053DD9464CC16C628140308AE5BC13B3058ABEC4970C721040747318
FailureExplanation: Analysis or notes documenting failures, blockers, or unproven claims.
FailureModeTags: constraint_violation; inconsistency; nonconvergence

Excerpt:
> # AGENT.md
> PROJECT: OperatorMath Lean proofs.
> RULES:
> Build and reason in axiom-free operator framework for arithmetic, logic, Gdel self-reference using inductive Trace with constructors void, delta, integrate, merge; emerge all from terminating confluent normalizationno external axioms, numerals, equality, Peano, classical like excluded middle. Define from scratch: theorems only, no lemmas; avoid sneaking Lean elements like Nat, DecidableEq, Bool/true/false, &&/|| operators, if-then-else, propext, classical, simp, rfl, by_cases, cases, injection, contradictionor code incompletes into unprovable void.
> CORE OBJECTS:
> - Trace, constructors: void | delta | integrate | merge
> - normalize (idempotent)
> - FixpointWitness, fixedpoint_of_idempotent
> - negation, Provable, , G
> TASK TYPES:

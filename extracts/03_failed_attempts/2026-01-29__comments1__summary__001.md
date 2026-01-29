Context: Failure catalog or commentary documenting incorrect assumptions or blockers.
Source: C:\Users\Moses\OpComp\MUST_Review\important_2\archive\comments1.md
SHA256: 4F0ED675C8E02487AB2C4DCFA910BD8729F1C1F0262CB07C7C0653C1424133F7
FailureExplanation: Failure catalog or commentary documenting incorrect assumptions or blockers.
FailureModeTags: unsupported_inference

Excerpt:
> Part A — Analysis and roadmap
> Conjecture (refined).
> C1. (Internal‑measure incompleteness, KO7 class). For any finite, purely operator‑based KO7‑style TRS that encodes arithmetic, and for any termination proof built only from measures definable inside the calculus (KO7 constructors, structural data, DM‑style multisets, ordinal expressions formed by KO7 combinators), there exists at least one reduction rule whose strict decrease is not provable by any such internal measure.
> C2. (Non‑coexistence at same level). In a terminating, confluent TRS where “provable” ≡ “reduces to ⊤”, same‑level quotation + diagonalization yielding “this term is not provable” cannot coexist with global SN. Reason: normalization decides reachability to ⊤; fixed‑target reachability is decidable; diagonalization plus decidable provability yields the classic trap. One of {SN, same‑level Prov, diagonalization strength} must be weakened or stratified.
> What “measure definable using only the operators” means.
> A measure is operator‑definable if it is assembled from KO7 constructors and their structural data without external predicates: sizes, tuples, multisets over subterms, path orders from a precedence on KO7 symbols, and ordinal expressions built as KO7‑terms. No semantic oracles, no external arithmetic. Proof obligations allowed: well‑foundedness of the chosen base order, monotonicity in all KO7 contexts, per‑rule “RHS pieces < LHS redex” checks.
> Base‑theory parameter.
> Internal: proofs carried out within the same KO7 equational calculus. Strongest claim, hardest to obtain; supports C2 blocker.
> PRA: primitive recursive arithmetic. Many TRS techniques formalize here; independence via hydra/Goodstein already bites below PA.
> PA: Peano Arithmetic. Goodstein and hydra terminations are true but unprovable in PA; any KO7 whose internal methods interpret within PA cannot prove those terminations.
> Tighter statement: choose the weakest theory T that soundly formalizes your internal method class; then “there exists a KO7‑rule whose termination is true but not provable in T”.
> Decidability link.

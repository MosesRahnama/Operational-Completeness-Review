Context: File review extract.
Source: C:\Users\Moses\OpComp\MUST_Review\Termination_Consolidation.md
SHA256: 8E8751F4882ADCF978CDF86ACFDBA7265E831CD00EAE24F191198D5761348579
FailureExplanation: Reference or planning document; not a proof artifact.
FailureModeTags: 

Excerpt:
> # Termination - KO7 Lex3, MPO, and Hybrid Summary
> This note consolidates the current termination strategy for the KO7-safe kernel and points to the audited lemmas in code. It mirrors the high-level document in `project_main_docs/Termination_Consolidation.md` and keeps a short, implementation-oriented index here.
> - Canonical reference: `project_main_docs/Termination_Consolidation.md` (this Meta_md page is a concise pointer/summary).
> - Code home: `OperatorKernelO6/Meta/Termination_KO7.lean` (KO7 Lex3 + MPO + Hybrid), plus `OperatorKernelO6/Meta/Termination.lean` for mu/mu-kappa tooling.
> ## KO7 triple measure (mu3) and Lex3
> - Measure: mu3(t) = (delta(t), (kappaM(t), mu(t))) where:
> - delta(t) = 1 only for top-level `rec b s (delta n)`, else 0.
> - kappaM is a multiset of recursion weights (DershowitzManna order).
> - mu is the ordinal measure used throughout MetaSN.
> - Order: Lex3 = lexicographic on (Nat, Multiset-DM, Ordinal).

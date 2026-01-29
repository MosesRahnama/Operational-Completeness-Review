Context: Background research memo; not a proof artifact.
Source: C:\Users\Moses\OpComp\MUST_Review\important_2\Research\Session_3_Independence_Witnesses_Goodstein_Hydra.md
SHA256: C9629013BE87705A72F601268FDF6848C121F8C594F1290130F8481921535141
FailureExplanation: Background research memo; not a proof artifact.
FailureModeTags: 

Excerpt:
> # Session 3 Independence Witnesses Goodstein Hydra
> *Converted from: Session 3 — Independence Witnesses_ Goodstein & Hydra.docx*
> ---
> # Session 3 — Independence Witnesses: Goodstein & Hydra
> In this session, we extract finite term rewrite systems (TRSs) encoding Goodstein sequences and Kirby–Paris’s Hydra battle, and explore which termination proving techniques can orient them. These examples are known to outrun “orders-only” methods (like simple lexicographic or multiset path orders) and require more powerful techniques such as AC-compatible orders or ordinal interpretations. We provide the TRS listings, discuss orientation attempts (plain MPO/LPO, then AC-MPO or labeling, then ordinal interpretations), and identify the first level of technique that succeeds for each. We also include evidence from termination tools (TTT2, AProVE) logs and certification, and explain why Peano Arithmetic (PA) cannot prove termination of these rewrite systems (Goodstein’s theorem and the Hydra battle are true but independent of PA[1][2]).
> ## Extracted Finite TRSs (Toy Encodings)
> To ground the discussion, we first extract two simple TRSs from the provided Lean files. These “toy” systems isolate core patterns from Goodstein’s sequence and the Hydra game:
> ### Goodstein Base-Change TRS (Toy Example)
> From GoodsteinCore.lean, we extract a minimal TRS modeling a base change with decrement of an exponent. There is one rule, where the base is incremented (S(b) represents  ) as the exponent (encoded as successors s(t)) is decreased:
> SIG  S/1, Z/0, s/1, state/2.
> RULES
> state(b, s(t)) -> state(S(b), t).

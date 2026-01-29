Context: Failure catalog or commentary documenting incorrect assumptions or blockers.
Source: C:\Users\Moses\OpComp\MUST_Review\important_2\WrongPredictions.md
SHA256: D9F86BA2A2E3C6FE45C850C941386B103117C3AE55D7137C0127A2F50DCF4376
FailureExplanation: Failure catalog or commentary documenting incorrect assumptions or blockers.
FailureModeTags: unsupported_inference

Excerpt:
> # Wrong Predictions and Simulated Failures (STRICT EXECUTION CONTRACT)
> This index catalogs concrete places where prior AI reasoning went wrong (or relied on unsound/global claims), with file:line anchors and brief notes.
> ## Quick map (RP → RE → Sim → Fix)
> | RP | Short description | RE | Simulation (section) | Primary fix anchor |
> |---|---|---|---|---|
> | RP-1 | Global rec_succ domination | RE-1 | Sim §5 (KO7 δ-flag P1), narrative | KO7 μ3 δ=1→0 (Termination_KO7) |
> | RP-2 | κ equality at rec_succ | RE-2 | (branch-split narrative) | κ-drop or δ primary (SN/KO7) |
> | RP-3 | Right-add strictness hazard | RE-3 | Sim §3 note | Guarded add/principal-add only |
> | RP-4 | Duplication under additive measure | RE-4 | Sim §5 DM/μ mapping | DM on κᴹ; μ-right on tie |
> | RP-6 | μ s vs μ(δ n) global comparison | RE-5 | Sim §4 counterexample | Avoid cross-parameter global bounds |
> ## RP-1: Global domination for rec_succ — not provable universally
> Evidence (bound left as sorry):

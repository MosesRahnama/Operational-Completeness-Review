Context: File review extract.
Source: C:\Users\Moses\OpComp\MUST_Review\Termination_Plan.md
SHA256: B8BFB62F6D17B9B2E2CE0D1894D01A7C6BF4237994FB85EC746AD2A30813C5E2
FailureExplanation: Analysis or notes documenting failures, blockers, or unproven claims.
FailureModeTags: constraint_violation; proof_obligation_stuck; nonconvergence

Excerpt:
> #   Meta-level Strong-Normalization Cookbook
> This file is **pure comments**.  Every unfinished lemma in `Termination_C.lean` is
> listed once, followed by an *assembly script*  a numbered sequence of micro-steps
> that a trivial dumb agent can follow without creativity.
> ---
> ## Legend
>  copy-pattern X:Y  = duplicate the proof fragment that sits in file `X`
> around line `Y` (only rename variables).
>  `tools/ordinal-toolkit.md n`  = lemma is guaranteed to exist there.
>  `ring`/`linarith` = allowed tactics.

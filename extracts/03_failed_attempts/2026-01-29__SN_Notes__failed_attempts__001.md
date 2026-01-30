Purpose: Evidence extract (failed_attempts/analysis) documenting a failure or relevance; Duplicate of FRV2-0084.
Contents: Metadata header + excerpt from the source file.
Context: Excerpt from SN_Notes showing AI attempts to fix the rec_succ termination proof, including the kappa case-split discovery and later reversal about blocked mu arguments.
Source: Legacy/MetaMD_Archive/SN_Notes.md
SHA256: C398C9A6CC524D9A22B9AC5461C75D583DAA9F83E768849060015AABF78B1E86
FailureExplanation: The notes show repeated patch proposals that first claim a clean fix, then later admit the delta-case and mu-drop require a forbidden bound, illustrating self-contradiction and repair loops.
FailureModeTags: inconsistency; case_split_omission; nonconvergence

Excerpt:
> It turns out the equality we were trying to assert is only definitionally
> true when `n` is not a `delta _`. Lean?s definition of `kappa` is:
> 
> | .rec? b s (.delta n) => ? + 1      -- successor case
> | .rec? b s n          => ?          -- all other cases
> 
> So a single lemma of the form
> 
> @[simp] lemma kappa_rec (b s n) :
>   kappa (rec? b s n) = max (max (kappa b) (kappa s)) (kappa n)
> 
> CANNOT hold when `n` itself is `delta _` ? that branch is off by `+1`.
> That?s why `rfl` (and every rewrite we tried) keeps failing.
> 
> ...
> 
> After those edits:
> 
> lake build OperatorKernelO6.Meta.SN_Final
> ? Build finished successfully
> 
> ...
> 
> I tried to implement the large-scale patch immediately, but compilation shows three substantive blockers:
> 
> 1. kappa equality in the ?-sub-case is in fact NOT provable by simp ...
> 2. MetaSN.mu_lt_rec_succ requires the external domination hypothesis ...
> 3. Nat.max_le is a proposition-valued lemma ...
> 
> Because the old ??-drop on ?-case? path re-introduces exactly the rec_succ_bound-style assumption the project rules forbid, the right fix is:
> 
> ? Stay with the lexicographic ?/? measure,
> ? but make ? pick up an extra bump (+2) ...





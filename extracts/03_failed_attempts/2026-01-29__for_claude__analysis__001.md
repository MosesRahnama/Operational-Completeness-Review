Context: Prescriptive fix script for Claude_SN that relies on a rec_succ bound lemma the project marks as impossible.
Source: C:\Users\Moses\OpComp\MUST_Review\Legacy\MetaMD_Archive\for_claude.md
SHA256: E584CED948532E69E8DCF2CB19936CFE50F1F4CA73B0B4BE28CDF9FE77100B14
FailureExplanation: The instructions require a forbidden rec_succ_bound ?-domination assumption, violating project constraints.
FailureModeTags: constraint_violation; unsupported_inference

Excerpt:
> | _, _, Step.R_rec_succ b s n => by
>   cases n with
>   | delta m =>
>       -- Use your boundful ?-lemma from Meta.Termination (green).
>       -- If it?s available as a wrapper:
>       -- have h? := MetaSN.mu_lt_rec_succ b s m MetaSN.rec_succ_bound
>       -- Or use the more primitive:
>       -- have h? := MetaSN.mu_merge_lt_rec (b:=b) (s:=s) (n:=m) MetaSN.rec_succ_bound
>       ...

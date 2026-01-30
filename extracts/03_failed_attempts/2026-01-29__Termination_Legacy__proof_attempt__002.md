Purpose: Evidence extract (failed_attempts/proof-attempt) documenting a failure or relevance; The rec_succ lemma depends on an external bound (h_mu_rec?_bound), so the core inequality is assumed rather than proved.
Contents: Metadata header + excerpt from the source file.
Context: Excerpt showing rec_succ lemma requiring an external bound assumption.
Source: MUST_Review/MetaMD_Archive/Termination_Legacy.md
SHA256: 655B548B733071679BE67E8DEF4D67BBFE9FA500A7C014C29CA00F2BAA29960E
FailureExplanation: The rec_succ lemma depends on an external bound (h_mu_recΔ_bound), so the core inequality is assumed rather than proved.
FailureModeTags: unsupported_inference

Excerpt:
> @[simp] lemma mu_lt_rec_succ (b s n : Trace)
>   (h_mu_recΔ_bound : omega0 ^ (mu n + mu s + (6 : Ordinal)) + omega0 * (mu b + 1) + 1 + 3 <
>                      (omega0 ^ (5 : Ordinal)) * (mu n + 1) + 1 + mu s + 6) :
>   mu (app s (recΔ b s n)) < mu (recΔ b s (delta n)) := by
>   simpa using mu_merge_lt_rec h_mu_recΔ_bound






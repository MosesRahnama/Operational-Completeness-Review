Purpose: Evidence extract (failed_attempts/proof-attempt) documenting a failure or relevance; Duplicate of Legacy\\MetaMD_Archive\\SN_Delta.md.
Contents: Metadata header + excerpt from the source file.
Context: Excerpt from SN_Delta showing the μ̂_decreases proof with an explicit `sorry` and an unreachable-branch assertion.
Source: Legacy/MetaMD_Archive/SN_Delta.md
SHA256: 2FBEEBA4954B4F8731BF05022A30EE62159BDD4186B6B9030A04D0DF7A24FDF4
FailureExplanation: The proof contains a `sorry` in the rec_zero case and asserts unreachable branches, leaving the SN argument incomplete.
FailureModeTags: proof_obligation_stuck; unsupported_inference

Excerpt:
> lemma μ̂_decreases : ∀ {a b : Trace}, Step a b → LexΔμ (μ̂ b) (μ̂ a)
> | _, _, R_int_delta t =>
>     by
>       unfold μ̂ LexΔμ; apply Prod.Lex.left
>       simp [d]; exact Nat.succ_pos _
> | _, _, R_merge_void_left t =>
>     lift_mu_drop (MetaSN.mu_lt_merge_void_left t) (by simp [d])
> | _, _, R_merge_void_right t =>
>     lift_mu_drop (MetaSN.mu_lt_merge_void_right t) (by simp [d])
> | _, _, R_merge_cancel t =>
>     by
>       by_cases h : d t = 0
>       · -- `d` equal, rely on μ drop
>         have : d (merge t t) = d t := by simp [d]
>         simpa [this] using
>           lift_mu_drop (MetaSN.mu_lt_merge_cancel t) this
>       · -- `d` strictly drops because we remove one copy of `t`
>         unfold μ̂ LexΔμ; apply Prod.Lex.left
>         -- d(merge t t) = d t + d t > d t  ⇒ want opposite direction (<)
>         -- We need `d merge` < `d t`.  Actually merge duplicates, so we must
>         -- rely on μ-drop branch (previous). The alternative branch above
>         -- already used μ-drop. This branch is unreachable.
>         exact (False.elim (by cases h (rfl)))
> | _, _, R_rec_zero b s =>
>     by
>       have hμ := MetaSN.mu_lt_rec_base b s
>       by_cases h : d s = 0
>       · -- `d` equal, use μ drop
>         have : d (recΔ b s void) = d b := by simp [d, h]
>         simpa [this] using lift_mu_drop hμ this
>       · -- `d` drops because `void` carries no delta, but `recΔ` gets removed
>         unfold μ̂ LexΔμ; apply Prod.Lex.left
>         -- compute delta counts explicitly
>         have : d (recΔ b s void) = d b + d s := by simp [d]
>         have : d (recΔ b s void) < d (recΔ b s (delta void)) := by
>           -- one extra delta in the hypothetical source; show strict drop 1
>           simp [d] at this; sorry
> | _, _, R_rec_succ b s n =>
>     by
>       unfold μ̂ LexΔμ; apply Prod.Lex.left
>       -- d(recΔ … (delta n)) = d b + d s + d n + 1
>       -- d(app s (recΔ … n)) = d s + d b + d s + d n
>       -- Hence drop by 1 when d s = 0 else equal, rely on μ drop
>       have : d (app s (recΔ b s n)) + 1 = d (recΔ b s (delta n)) := by
>         simp [d, Nat.add_comm, Nat.add_left_comm, Nat.add_assoc]
>       have : d (app s (recΔ b s n)) < d (recΔ b s (delta n)) := by
>         -- natural numbers, drop by one
>         have : d (app s (recΔ b s n)) + 1 = _ := this
>         have := Nat.lt_succ_self (d (app s (recΔ b s n)))
>         simpa [this] using this
>       simpa using this
> | _, _, R_eq_refl t =>
>     by
>       unfold μ̂ LexΔμ; apply Prod.Lex.left
>       by_cases h : d t = 0
>       · simp [d, h]
>       · have : 0 < d t := by
>           cases h' : d t with
>           | zero => cases h rfl
>           | succ _ => simpa using Nat.succ_pos _
>         simpa [d] using this
> | _, _, R_eq_diff a b hab =>
>     lift_mu_drop (MetaSN.mu_lt_eq_diff a b) (by simp [d])






Context: Excerpt from Claude_SN showing the rec_succ case with a sorry for the μ domination bound.
Source: C:\Users\Moses\OpComp\MUST_Review\Legacy\Meta_Lean_Archive\Claude_SN.lean
SHA256: 1CBACF8DEEBA9605CD0627070476A27BB892F1625F5D3BC4274CB5506EF82558
FailureExplanation: The rec_succ proof inserts a `sorry` for the required μ bound, so the lex termination proof is incomplete.
FailureModeTags: proof_obligation_stuck; unsupported_inference

Excerpt:
> | _, _, Step.R_rec_succ b s n => by
>     cases n with
>     | delta m =>
>       -- Need the bound hypothesis for mu_lt_rec_succ
>       -- This is the known problematic bound that cannot be proved
>       -- We leave it as sorry for now as it's a known research problem
>       have h_bound : omega0 ^ (MetaSN.mu (delta m) + MetaSN.mu s + (6 : Ordinal)) + omega0 * (MetaSN.mu b + 1) + 1 + 3 <
>                      (omega0 ^ (5 : Ordinal)) * (MetaSN.mu (delta m) + 1) + 1 + MetaSN.mu s + 6 := sorry
>       have hμ := MetaSN.mu_lt_rec_succ b s (delta m) h_bound
>       have hk : kappa (merge s (recΔ b s (delta m))) = kappa (recΔ b s (delta (delta m))) := by
>         have hs_le : kappa s ≤ Nat.max (Nat.max (kappa b) (kappa s)) (kappa m) := by
>           exact le_trans (Nat.le_max_right _ _) (Nat.le_max_left _ _)
>         have h_rec : kappa (recΔ b s (delta m)) = Nat.max (Nat.max (kappa b) (kappa s)) (kappa m) + 1 := by
>           simp [kappa]
>         -- Fix max association for the goal
>         have hs_le' : kappa s ≤ Nat.max (kappa b) (Nat.max (kappa s) (kappa m)) := by
>           -- reorder ((κ b).max (κ s)).max (κ m) to κ b .max (κ s .max κ m)
>           simpa [Nat.max_assoc, Nat.max_left_comm, Nat.max_comm] using hs_le
>         have hs_le_succ : kappa s ≤ Nat.max (kappa b) (Nat.max (kappa s) (kappa m)) + 1 :=
>           Nat.le_trans hs_le' (Nat.le_succ _)
>         simpa [kappa_merge, h_rec, Nat.max_eq_right hs_le_succ]
>       exact drop_right hμ hk
>     | void =>
>       let base := Nat.max (Nat.max (kappa b) (kappa s)) 0
>       have hs_le : kappa s ≤ base :=
>         le_trans (Nat.le_max_right _ _) (Nat.le_max_left _ _)
>       have h_merge : kappa (merge s (recΔ b s void)) = base := by
>         simp [kappa, base]
>       have h_rec : kappa (recΔ b s (delta void)) = base + 1 := by
>         simp [kappa, base]
>       apply drop_left
>       simpa [h_merge, h_rec] using Nat.lt_succ_self base
>     | integrate t =>
>       let base := Nat.max (Nat.max (kappa b) (kappa s)) (kappa t)
>       have hs_le : kappa s ≤ base :=
>         le_trans (Nat.le_max_right _ _) (Nat.le_max_left _ _)
>       have h_merge : kappa (merge s (recΔ b s (integrate t))) = base := by
>         simp [kappa, base]
>       have h_rec : kappa (recΔ b s (delta (integrate t))) = base + 1 := by
>         simp [kappa, base]
>       apply drop_left
>       simpa [h_merge, h_rec] using Nat.lt_succ_self base
>     | merge t1 t2 =>
>       let base := Nat.max (Nat.max (kappa b) (kappa s)) (Nat.max (kappa t1) (kappa t2))
>       have hs_le : kappa s ≤ base :=
>         le_trans (Nat.le_max_right _ _) (Nat.le_max_left _ _)
>       have h_merge : kappa (merge s (recΔ b s (merge t1 t2))) = base := by
>         simp [kappa, base]
>       have h_rec : kappa (recΔ b s (delta (merge t1 t2))) = base + 1 := by
>         simp [kappa, base]
>       apply drop_left
>       simpa [h_merge, h_rec] using Nat.lt_succ_self base
>     | recΔ t1 t2 t3 =>
>       let base := Nat.max (Nat.max (kappa b) (kappa s)) (kappa (recΔ t1 t2 t3))
>       have hs_le : kappa s ≤ base :=
>         le_trans (Nat.le_max_right _ _) (Nat.le_max_left _ _)
>       have h_merge : kappa (merge s (recΔ b s (recΔ t1 t2 t3))) = base := by
>         simp [kappa, base]
>       have h_rec : kappa (recΔ b s (delta (recΔ t1 t2 t3))) = base + 1 := by
>         simp [kappa, base]
>       apply drop_left
>       simpa [h_merge, h_rec] using Nat.lt_succ_self base
>     | eqW t1 t2 =>
>       let base := Nat.max (Nat.max (kappa b) (kappa s)) (Nat.max (kappa t1) (kappa t2))
>       have hs_le : kappa s ≤ base :=
>         le_trans (Nat.le_max_right _ _) (Nat.le_max_left _ _)
>       have h_merge : kappa (merge s (recΔ b s (eqW t1 t2))) = base := by
>         simp [kappa, base]
>       have h_rec : kappa (recΔ b s (delta (eqW t1 t2))) = base + 1 := by
>         simp [kappa, base]
>       apply drop_left
>       simpa [h_merge, h_rec] using Nat.lt_succ_self base

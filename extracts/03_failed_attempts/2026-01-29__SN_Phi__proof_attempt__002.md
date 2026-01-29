Context: Excerpt showing an admitted subgoal in the Φ-based rec_succ proof.
Source: C:\Users\Moses\OpComp\MUST_Review\MetaMD_Archive\SN_Phi.md
SHA256: D6B1958715DF376853BC18CCAAF72A425D158B8DEBD8C5CF4896606265AC0050
FailureExplanation: The file contains an admit and relies on heavy automation (norm_num/linarith), so the Φ-based SN proof is incomplete.
FailureModeTags: constraint_violation; proof_obligation_stuck; unsupported_inference

Excerpt:
> have one_lt : (1 : Ordinal) < A := by
>   -- A is a principal tower; certainly > 1
>   have : (0 : Ordinal) < A := by simpa [hA] using (Ordinal.opow_pos (a := omega0) (b := _)
>     omega0_pos)
>   exact lt_of_le_of_lt (by norm_num : (1 : Ordinal) ≤ 2) (lt_trans (by exact ?h1) this)
>   admit
> exact prin (by simpa using hsum) (by exact one_lt)

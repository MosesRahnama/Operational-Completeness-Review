Context: Excerpt from SN_Opus showing μ₂ definition and the rec_succ decrease proof relying on linarith and max_def case splits.
Source: C:\Users\Moses\OpComp\MUST_Review\Legacy\MetaMD_Archive\SN_Opus.md
SHA256: D1A02D18B85E10D0C6C1BD9C7200D167EE71C9C7A65F3FCD87CFE3E7B20F9B4F
FailureExplanation: The proof sketch uses linarith and informal ordinal inequalities to assert μ₂ decreases, which is not justified in the project constraints.
FailureModeTags: constraint_violation; unsupported_inference

Excerpt:
> def μ₂ (t : Trace) : Ordinal :=
>   Ordinal.omega0 ^ (Ordinal.omega0 ^ (baseLayer t))
> 
> /-! ## 3. Key Lemmas for Ordinal Arithmetic
> 
> These lemmas establish the ordinal relationships needed for termination proofs.
> -/
> 
> lemma omega_exp_monotone {α β : Ordinal} (h : α < β) :
>     Ordinal.omega0 ^ α < Ordinal.omega0 ^ β := by
>   exact Ordinal.power_lt_power_right Ordinal.omega0_pos h
> 
> lemma double_exp_monotone {α β : Ordinal} (h : α < β) :
>     Ordinal.omega0 ^ (Ordinal.omega0 ^ α) < Ordinal.omega0 ^ (Ordinal.omega0 ^ β) := by
>   apply omega_exp_monotone
>   exact omega_exp_monotone h
> 
> lemma baseLayer_delta_growth (t : Trace) :
>     baseLayer t < baseLayer (delta t) := by
>   simp [baseLayer]
>   have h : baseLayer t < Ordinal.omega0 * (1 + baseLayer t) := by
>     rw [Ordinal.mul_one_add]
>     simp [Ordinal.omega0_pos]
>     exact Ordinal.lt_add_of_pos_left _ Ordinal.omega0_pos
>   exact h
> 
> lemma baseLayer_merge_bound (t1 t2 : Trace) :
>     baseLayer (merge t1 t2) ≤ 1 + max (baseLayer t1) (baseLayer t2) := by
>   simp [baseLayer]
> 
> /-! ## 4. Strict Decrease Proofs for Each Kernel Rule
> 
> For each of the 8 kernel rules, we prove that μ₂ strictly decreases.
> -/
> 
> -- R_int_delta: integrate (delta t) → void
> theorem mu2_decrease_int_delta (t : Trace) :
>     μ₂ void < μ₂ (integrate (delta t)) := by
>   unfold μ₂
>   apply double_exp_monotone
>   simp [baseLayer]
>   have h : 0 < Ordinal.omega0 * (1 + baseLayer t) := by
>     exact Ordinal.mul_pos Ordinal.omega0_pos (Ordinal.one_add_pos _)
>   exact h
> 
> -- R_merge_void_left: merge void t → t
> theorem mu2_decrease_merge_void_left (t : Trace) :
>     μ₂ t < μ₂ (merge void t) := by
>   unfold μ₂
>   apply double_exp_monotone
>   simp [baseLayer]
>   exact Nat.lt_one_add _
> 
> -- R_merge_void_right: merge t void → t
> theorem mu2_decrease_merge_void_right (t : Trace) :
>     μ₂ t < μ₂ (merge t void) := by
>   unfold μ₂
>   apply double_exp_monotone
>   simp [baseLayer]
>   exact Nat.lt_one_add _
> 
> -- R_merge_cancel: merge t t → t
> theorem mu2_decrease_merge_cancel (t : Trace) :
>     μ₂ t < μ₂ (merge t t) := by
>   unfold μ₂
>   apply double_exp_monotone
>   simp [baseLayer]
>   exact Nat.lt_one_add _
> 
> -- R_rec_zero: recΔ b s void → b
> theorem mu2_decrease_rec_zero (b s : Trace) :
>     μ₂ b < μ₂ (recΔ b s void) := by
>   unfold μ₂
>   apply double_exp_monotone
>   simp [baseLayer]
>   linarith
> 
> -- R_rec_succ: recΔ b s (delta n) → merge s (recΔ b s n)
> -- This is the crucial case that motivated the double-exponent construction
> theorem mu2_decrease_rec_succ (b s n : Trace) :
>     μ₂ (merge s (recΔ b s n)) < μ₂ (recΔ b s (delta n)) := by
>   unfold μ₂
>   apply double_exp_monotone
>   simp [baseLayer]
>   -- Key insight: delta wrapper creates exponential gap
>   have h1 : baseLayer n < Ordinal.omega0 * (1 + baseLayer n) := by
>     rw [Ordinal.mul_one_add]
>     exact Ordinal.lt_add_of_pos_left _ Ordinal.omega0_pos
>   have h2 : 2 + baseLayer n + baseLayer s + baseLayer b <
>             2 + (Ordinal.omega0 * (1 + baseLayer n)) + baseLayer s + baseLayer b := by
>     linarith [h1]
>   -- Merge bound ensures the result is smaller
>   have h3 : 1 + max (baseLayer s) (2 + baseLayer n + baseLayer s + baseLayer b) ≤
>             2 + baseLayer n + baseLayer s + baseLayer b := by
>     simp [max_def]
>     by_cases h : baseLayer s ≤ 2 + baseLayer n + baseLayer s + baseLayer b
>     · simp [h]
>       linarith
>     · simp [h]
>       linarith
>   linarith [h2, h3]

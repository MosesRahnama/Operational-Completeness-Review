Purpose: Evidence extract (failed_attempts/proof-attempt) documenting a failure or relevance; The file uses linarith/norm_num and contains an admit in the rec_succ proof, so the ?-based SN proof is incomplete.
Contents: Metadata header + excerpt from the source file.
Context: Excerpt from SN_Phi showing the Φ measure and the rec_succ proof containing an admit.
Source: Legacy/MetaMD_Archive/SN_Phi.md
SHA256: 4B7F7E6E2A5B0D1D2546E5CE5D8A0B443D5B316C76E9E9FE2F6C6B2C5BCA1776
FailureExplanation: The file uses linarith/norm_num and contains an admit in the rec_succ proof, so the Φ-based SN proof is incomplete.
FailureModeTags: constraint_violation; proof_obligation_stuck; unsupported_inference

Excerpt:
> noncomputable def phi : Trace → Ordinal
> | void          => 0
> | delta t       => (omega0 ^ (5 : Ordinal)) * (phi t + 1) + 1
> | integrate t   => (omega0 ^ (4 : Ordinal)) * (phi t + 1) + 1
> | merge a b     => (omega0 ^ (3 : Ordinal)) * (phi a + 1)
>                     + (omega0 ^ (2 : Ordinal)) * (phi b + 1) + 1
> | recΔ b s n    => omega0 ^ ((omega0 ^ (5 : Ordinal)) * (phi n + 1) + phi s + 6)
>                     + omega0 * (phi b + 1) + 1
> | eqW a b       => omega0 ^ (phi a + phi b + 9) + 1
> 
> /-!
> Auxiliary generic ordinal lemmas are re-used from `MetaSN` (Termination.lean):
> - termA_le (ω^3 * (x+1) ≤ ω^(x+4))
> - termB_le (ω^2 * (x+1) ≤ ω^(x+3))
> and the additive principal lemma `Ordinal.principal_add_omega0_opow` via wrappers.
> -/
> 
> open MetaSN
> 
> /-- merge void left: Φ strictly larger than argument. -/
> theorem phi_lt_merge_void_left (t : Trace) :
>   phi t < phi (merge void t) := by
>   -- phi(merge void t) = ω^3*(Φ void +1) + ω^2*(Φ t + 1) + 1
>   have h0 : phi t < phi t + 1 :=
>     (Order.lt_add_one_iff (x := phi t) (y := phi t)).2 le_rfl
>   have pos2 : 0 < omega0 ^ (2 : Ordinal) :=
>     Ordinal.opow_pos (a := omega0) (b := (2 : Ordinal)) omega0_pos
>   have hmono : phi t + 1 ≤ (omega0 ^ (2 : Ordinal)) * (phi t + 1) := by
>     simpa using (Ordinal.le_mul_right (a := phi t + 1) (b := omega0 ^ (2 : Ordinal)) pos2)
>   have h1 : phi t < (omega0 ^ (2 : Ordinal)) * (phi t + 1) := lt_of_lt_of_le h0 hmono
>   have hpad :
>       (omega0 ^ (2 : Ordinal)) * (phi t + 1) ≤
>       (omega0 ^ (3 : Ordinal)) * (phi void + 1) + (omega0 ^ (2 : Ordinal)) * (phi t + 1) := by
>     exact Ordinal.le_add_left _ _
>   have hfin : phi t < (omega0 ^ (3 : Ordinal)) * (phi void + 1)
>                     + (omega0 ^ (2 : Ordinal)) * (phi t + 1) :=
>     lt_of_lt_of_le h1 hpad
>   have : phi t < phi (merge void t) := by
>     simpa [phi, add_assoc] using (lt_of_lt_of_le hfin (le_add_of_nonneg_right (zero_le _)))
>   exact this
> 
> /-- merge void right: Φ strictly larger than argument. -/
> theorem phi_lt_merge_void_right (t : Trace) :
>   phi t < phi (merge t void) := by
>   -- symmetric to left
>   have h0 : phi t < phi t + 1 :=
>     (Order.lt_add_one_iff (x := phi t) (y := phi t)).2 le_rfl
>   have pos2 : 0 < omega0 ^ (2 : Ordinal) :=
>     Ordinal.opow_pos (a := omega0) (b := (2 : Ordinal)) omega0_pos
>   have hmono : phi t + 1 ≤ (omega0 ^ (2 : Ordinal)) * (phi t + 1) := by
>     simpa using (Ordinal.le_mul_right (a := phi t + 1) (b := omega0 ^ (2 : Ordinal)) pos2)
>   have h1 : phi t < (omega0 ^ (2 : Ordinal)) * (phi t + 1) := lt_of_lt_of_le h0 hmono
>   have hpad :
>       (omega0 ^ (2 : Ordinal)) * (phi t + 1) ≤
>       (omega0 ^ (3 : Ordinal)) * (phi t + 1) + (omega0 ^ (2 : Ordinal)) * (phi void + 1) := by
>     exact Ordinal.le_add_right _ _
>   have hfin : phi t < (omega0 ^ (3 : Ordinal)) * (phi t + 1)
>                     + (omega0 ^ (2 : Ordinal)) * (phi void + 1) :=
>     lt_of_lt_of_le h1 hpad
>   have : phi t < phi (merge t void) := by
>     simpa [phi, add_assoc] using (lt_of_lt_of_le hfin (le_add_of_nonneg_right (zero_le _)))
>   exact this
> 
> /-- merge cancel: Φ strictly larger than argument. -/
> theorem phi_lt_merge_cancel (t : Trace) :
>   phi t < phi (merge t t) := by
>   have h0 : phi t < phi t + 1 :=
>     (Order.lt_add_one_iff (x := phi t) (y := phi t)).2 le_rfl
>   have pos3 : 0 < omega0 ^ (3 : Ordinal) :=
>     Ordinal.opow_pos (a := omega0) (b := (3 : Ordinal)) omega0_pos
>   have hmono : phi t + 1 ≤ (omega0 ^ (3 : Ordinal)) * (phi t + 1) := by
>     simpa using (Ordinal.le_mul_right (a := phi t + 1) (b := omega0 ^ (3 : Ordinal)) pos3)
>   have h1 : phi t < (omega0 ^ (3 : Ordinal)) * (phi t + 1) := lt_of_lt_of_le h0 hmono
>   have pad :
>       (omega0 ^ (3 : Ordinal)) * (phi t + 1) ≤
>       (omega0 ^ (3 : Ordinal)) * (phi t + 1) + (omega0 ^ (2 : Ordinal)) * (phi t + 1) :=
>     Ordinal.le_add_right _ _
>   have h2 :
>       phi t < (omega0 ^ (3 : Ordinal)) * (phi t + 1)
>                  + (omega0 ^ (2 : Ordinal)) * (phi t + 1) :=
>     lt_of_lt_of_le h1 pad
>   simpa [phi, add_assoc] using (lt_of_lt_of_le h2 (le_add_of_nonneg_right (zero_le _)))
> 
> /-- eq refl: Φ(void) < Φ(eqW a a). -/
> theorem phi_void_lt_eq_refl (a : Trace) : phi void < phi (eqW a a) := by
>   simp [phi]
> 
> /-- integrate(delta t) dominates void. -/
> theorem phi_void_lt_integrate_delta (t : Trace) :
>   phi void < phi (integrate (delta t)) := by
>   simp [phi]
> 
> /-- eq diff: integrate(merge a b) < eqW a b. -/
> theorem phi_lt_eq_diff (a b : Trace) :
>   phi (integrate (merge a b)) < phi (eqW a b) := by
>   -- As in μ proof: bound head and tail under ω^(C+5), then lift by ω^4 to ω^(C+9)
>   set C : Ordinal := phi a + phi b with hC
>   have h_head : (omega0 ^ (3 : Ordinal)) * (phi a + 1) ≤ omega0 ^ (phi a + 4) := termA_le (x := phi a)
>   have h_tail : (omega0 ^ (2 : Ordinal)) * (phi b + 1) ≤ omega0 ^ (phi b + 3) := termB_le (x := phi b)
>   have h_lt1 : phi a + 4 < C + 5 := by
>     have h1 : phi a ≤ C := by simpa [hC] using Ordinal.le_add_right (phi a) (phi b)
>     have h2 : phi a + 4 ≤ C + 4 := add_le_add_right h1 4
>     have h3 : C + 4 < C + 5 := add_lt_add_left (by norm_num : (4 : Ordinal) < 5) C
>     exact lt_of_le_of_lt h2 h3
>   have h_lt2 : phi b + 3 < C + 5 := by
>     have h1 : phi b ≤ C := by simpa [hC, add_comm] using Ordinal.le_add_left (phi b) (phi a)
>     have h2 : phi b + 3 ≤ C + 3 := add_le_add_right h1 3
>     have h3 : C + 3 < C + 5 := add_lt_add_left (by norm_num : (3 : Ordinal) < 5) C
>     exact lt_of_le_of_lt h2 h3
>   have h1_pow : (omega0 ^ (3 : Ordinal)) * (phi a + 1) < omega0 ^ (C + 5) := by
>     exact lt_of_le_of_lt h_head (opow_lt_opow_right h_lt1)
>   have h2_pow : (omega0 ^ (2 : Ordinal)) * (phi b + 1) < omega0 ^ (C + 5) := by
>     exact lt_of_le_of_lt h_tail (opow_lt_opow_right h_lt2)
>   have h_fin : (2 : Ordinal) < omega0 ^ (C + 5) := by
>     have two_lt_omega : (2 : Ordinal) < omega0 := nat_lt_omega0 2
>     have omega_le : omega0 ≤ omega0 ^ (C + 5) := by
>       have one_le : (1 : Ordinal) ≤ C + 5 := by exact le_trans (by norm_num) (le_add_left _ _)
>       simpa [Ordinal.opow_one] using (Ordinal.opow_le_opow_right omega0_pos one_le)
>     exact lt_of_lt_of_le two_lt_omega omega_le
>   have pos : 0 < (C + 5) := by exact lt_of_le_of_lt (by exact (zero_le _)) (by norm_num)
>   have hsum :
>       (omega0 ^ (3 : Ordinal)) * (phi a + 1)
>         + ((omega0 ^ (2 : Ordinal)) * (phi b + 1) + 2)
>         < omega0 ^ (C + 5) := by
>     -- use additive principal under ω^(C+5)
>     have prin := Ordinal.principal_add_omega0_opow (C + 5)
>     -- first sum two parts below bound
>     have h_ab : (omega0 ^ (3 : Ordinal)) * (phi a + 1)
>                 + (omega0 ^ (2 : Ordinal)) * (phi b + 1)
>                 < omega0 ^ (C + 5) := prin h1_pow h2_pow
>     exact prin (by simpa using h_ab) h_fin
>   -- lift by ω^4 on left; compare to ω^(C+9) on right
>   have ω4pos : 0 < omega0 ^ (4 : Ordinal) := Ordinal.opow_pos (b := (4 : Ordinal)) omega0_pos
>   have h_mul : (omega0 ^ (4 : Ordinal)) *
>       ((omega0 ^ (3 : Ordinal)) * (phi a + 1) + ((omega0 ^ (2 : Ordinal)) * (phi b + 1) + 2))
>       < omega0 ^ (4 : Ordinal) * omega0 ^ (C + 5) :=
>     Ordinal.mul_lt_mul_of_pos_left hsum ω4pos
>   have h_collapse : omega0 ^ (4 : Ordinal) * omega0 ^ (C + 5) =
>       omega0 ^ (4 + (C + 5)) := by
>     simpa using (Ordinal.opow_add omega0 (4 : Ordinal) (C + 5)).symm
>   have : (omega0 ^ (4 : Ordinal)) *
>       ((omega0 ^ (3 : Ordinal)) * (phi a + 1) + ((omega0 ^ (2 : Ordinal)) * (phi b + 1) + 2))
>       < omega0 ^ (C + 9) := by
>     have := lt_of_lt_of_eq h_mul h_collapse
>     -- 4 + (C+5) = C + 9
>     have h_eq : (4 : Ordinal) + (C + 5) = C + 9 := by
>       simp [add_assoc, add_comm, add_left_comm]
>     simpa [h_eq] using this
>   -- rewrite in terms of phi
>   have hL : phi (integrate (merge a b)) =
>       (omega0 ^ (4 : Ordinal)) * ((omega0 ^ (3 : Ordinal)) * (phi a + 1)
>         + ((omega0 ^ (2 : Ordinal)) * (phi b + 1) + 2)) + 1 := by
>     simp [phi, add_assoc]
>   have hR : phi (eqW a b) = omega0 ^ (C + 9) + 1 := by
>     simp [phi, hC]
>   have hA1 :
>       (omega0 ^ (4 : Ordinal)) * ((omega0 ^ (3 : Ordinal)) * (phi a + 1)
>         + ((omega0 ^ (2 : Ordinal)) * (phi b + 1) + 2)) + 1 ≤
>       omega0 ^ (C + 9) := Order.add_one_le_of_lt this
>   have hfin :
>       (omega0 ^ (4 : Ordinal)) * ((omega0 ^ (3 : Ordinal)) * (phi a + 1)
>         + ((omega0 ^ (2 : Ordinal)) * (phi b + 1) + 2)) + 1 <
>       omega0 ^ (C + 9) + 1 :=
>     (Order.lt_add_one_iff (x := _ + 1) (y := omega0 ^ (C + 9))).2 hA1
>   simpa [hL, hR] using hfin
> 
> /-- rec_zero: Φ strictly increases across recΔ base → b. -/
> theorem phi_lt_rec_base (b s : Trace) :
>   phi b < phi (recΔ b s void) := by
>   -- use the ω·(Φ b + 1) tail to dominate Φ b, plus the huge head
>   have h1 : phi b < phi b + 1 := (Order.lt_add_one_iff (x := phi b) (y := phi b)).2 le_rfl
>   have h2 : phi b + 1 ≤ omega0 * (phi b + 1) := by
>     have : 0 < omega0 := omega0_pos
>     simpa using (Ordinal.le_mul_right (a := phi b + 1) (b := omega0) this)
>   have h3 : phi b < omega0 * (phi b + 1) := lt_of_lt_of_le h1 h2
>   have hpad : omega0 * (phi b + 1) ≤
>       omega0 ^ ((omega0 ^ (5 : Ordinal)) * (phi void + 1) + phi s + 6)
>       + omega0 * (phi b + 1) + 1 := by
>     have : omega0 * (phi b + 1) ≤ omega0 * (phi b + 1) + 1 :=
>       le_add_of_nonneg_right (zero_le _)
>     exact this.trans (le_add_of_nonneg_left (zero_le _))
>   have : phi b < phi (recΔ b s void) := by
>     have := lt_of_lt_of_le h3 hpad
>     simpa [phi] using this
>   exact this
> 
> /-- rec_succ: merge s (recΔ b s n) < recΔ b s (delta n). -/
> theorem phi_merge_lt_rec (b s n : Trace) :
>   phi (merge s (recΔ b s n)) < phi (recΔ b s (delta n)) := by
>   -- Let A = ω^(E(δ n, s)) be the head of the RHS
>   set A : Ordinal := omega0 ^ ((omega0 ^ (5 : Ordinal)) * (phi (delta n) + 1) + phi s + 6) with hA
>   -- 1) head bound: ω^3*(Φ s + 1) < A
>   have h_head1 : (omega0 ^ (3 : Ordinal)) * (phi s + 1) ≤ omega0 ^ (phi s + 4) := termA_le (x := phi s)
>   have h_head2 : phi s + 4 < phi s + 6 := by simpa using add_lt_add_left (by norm_num : (4 : Ordinal) < 6) (phi s)
>   have h_head3 : omega0 ^ (phi s + 4) < omega0 ^ (phi s + 6) := opow_lt_opow_right h_head2
>   have h_head4 : phi s + 6 ≤ (omega0 ^ (5 : Ordinal)) * (phi (delta n) + 1) + phi s + 6 := by
>     have : (0 : Ordinal) ≤ (omega0 ^ (5 : Ordinal)) * (phi (delta n) + 1) := zero_le _
>     simpa [add_comm, add_left_comm, add_assoc] using add_le_add_right this (phi s + 6)
>   have h_head : (omega0 ^ (3 : Ordinal)) * (phi s + 1) < A := by
>     have := lt_of_le_of_lt h_head1 h_head3
>     have : omega0 ^ (phi s + 4) < omega0 ^ ((omega0 ^ (5 : Ordinal)) * (phi (delta n) + 1) + phi s + 6) :=
>       lt_of_lt_of_le this (opow_le_opow_right omega0_pos h_head4)
>     simpa [hA] using this
>   -- 2) tail bound: ω^2*(Φ(recΔ b s n) + 1) < A
>   have h_tail1 : (omega0 ^ (2 : Ordinal)) * (phi (recΔ b s n) + 1)
>                 ≤ omega0 ^ (phi (recΔ b s n) + 3) := termB_le (x := phi (recΔ b s n))
>   -- Show phi(recΔ b s n) + 3 < (ω^5)*(phi(δ n)+1) + phi s + 6
>   have bump : phi (recΔ b s n) + 3 < (omega0 ^ (5 : Ordinal)) * (phi (delta n) + 1) + phi s + 6 := by
>     -- phi(recΔ b s n) = ω^(E(n,s)) + ω·(φ b +1) + 1 with E(n,s) = ω^5*(φ n +1) + φ s + 6
>     -- and E(δ n,s) = ω^5*(φ(δ n) + 1) + φ s + 6 ≥ E(n,s) + 3
>     -- Use monotonicity: φ(δ n) = ω^5*(φ n + 1) + 1 ⇒ ω^5*(φ(δ n)+1) ≥ ω^5*(ω^5*(φ n + 1) + 2)
>     -- hence E(δ n,s) dominates E(n,s) + 3.
>     -- We bound crudely using ≤ to place φ(recΔ b s n) + 3 below E(δ n,s).
>     have : (phi (recΔ b s n)) ≤ (omega0 ^ (5 : Ordinal)) * (phi n + 1) + phi s + 6 := by
>       -- drop the tail and take only the head exponent
>       have : omega0 ^ ((omega0 ^ (5 : Ordinal)) * (phi n + 1) + phi s + 6)
>                 ≤ omega0 ^ ((omega0 ^ (5 : Ordinal)) * (phi n + 1) + phi s + 6) := le_rfl
>       -- φ(recΔ) = head + tail ≤ head + (ω·(φ b +1)+1) ≤ head + head (coarse)
>       -- So φ(recΔ) ≤ 2·head ≤ ω·head ≤ ... < E(δn,s). For simplicity, use:
>       exact le_trans (le_add_of_nonneg_right (zero_le _)) (le_of_eq rfl)
>     have add3 : (phi (recΔ b s n)) + 3 ≤ (omega0 ^ (5 : Ordinal)) * (phi n + 1) + phi s + 9 :=
>       add_le_add_right this 3
>     have step : (omega0 ^ (5 : Ordinal)) * (phi n + 1) + phi s + 9 <
>                  (omega0 ^ (5 : Ordinal)) * (phi (delta n) + 1) + phi s + 6 := by
>       -- since φ(δ n) ≥ ω^5*(φ n + 1) + 1, multiplying by ω^5 on the left blows up
>       -- hence RHS exponent is much larger; adding finite 9 vs 6 is irrelevant.
>       -- We justify strictly by: (phi n + 1) < (phi (delta n) + 1) ⇒
>       -- (ω^5)*(phi n + 1) < (ω^5)*(phi(δ n) + 1)
>       have inc : (phi n + 1) < (phi (delta n) + 1) := by
>         -- φ(δ n) = ω^5*(φ n + 1) + 1 > φ n
>         have : phi n < phi (delta n) := by
>           -- immediate from definition of phi(delta n)
>           have h0 : phi n < phi n + 1 := (Order.lt_add_one_iff (x := phi n) (y := phi n)).2 le_rfl
>           have : phi n + 1 ≤ (omega0 ^ (5 : Ordinal)) * (phi n + 1) + 1 := by
>             have h1 : phi n + 1 ≤ (omega0 ^ (5 : Ordinal)) * (phi n + 1) := by
>               have hone : (1 : Ordinal) ≤ omega0 := by simpa using one_le_omega0
>               have hωle : omega0 ≤ omega0 ^ (5 : Ordinal) := by
>                 simpa [Ordinal.opow_one] using (Ordinal.opow_le_opow_right omega0_pos (by norm_num : (1 : Ordinal) ≤ (5 : Ordinal)))
>               have hge : (1 : Ordinal) ≤ omega0 ^ (5 : Ordinal) := le_trans hone hωle
>               simpa [one_mul] using (mul_le_mul_right' hge (phi n + 1))
>             exact le_trans h1 (le_add_of_nonneg_right (zero_le _))
>           exact lt_of_lt_of_le h0 this
>         exact add_lt_add_right this 1
>       have : (omega0 ^ (5 : Ordinal)) * (phi n + 1) < (omega0 ^ (5 : Ordinal)) * (phi (delta n) + 1) :=
>         Ordinal.mul_lt_mul_of_pos_left inc (Ordinal.opow_pos (a := omega0) (b := (5 : Ordinal)) omega0_pos)
>       -- add phi s + 6 preserves strictness
>       exact add_lt_add_right (add_lt_add_right this (phi s)) 6
>     exact lt_of_le_of_lt add3 step
>   have h_tail2 : (omega0 ^ (2 : Ordinal)) * (phi (recΔ b s n) + 1)
>                   < omega0 ^ ((omega0 ^ (5 : Ordinal)) * (phi (delta n) + 1) + phi s + 6) := by
>     exact lt_of_le_of_lt h_tail1 (opow_lt_opow_right bump)
>   -- 3) combine head+tail+1 below A by additive principal
>   have prin := Ordinal.principal_add_omega0_opow (((omega0 ^ (5 : Ordinal)) * (phi (delta n) + 1) + phi s + 6))
>   have sum_lt : (omega0 ^ (3 : Ordinal)) * (phi s + 1)
>                   + ((omega0 ^ (2 : Ordinal)) * (phi (recΔ b s n) + 1) + 1)
>                   < A := by
>     have hsum := prin h_head h_tail2
>     have one_lt : (1 : Ordinal) < A := by
>       -- A is a principal tower; certainly > 1
>       have : (0 : Ordinal) < A := by simpa [hA] using (Ordinal.opow_pos (a := omega0) (b := _)
>         omega0_pos)
>       exact lt_of_le_of_lt (by norm_num : (1 : Ordinal) ≤ 2) (lt_trans (by exact ?h1) this)
>       admit
>     exact prin (by simpa using hsum) (by exact one_lt)
>   -- 4) RHS = A + ω·… + 1 > A > LHS
>   have rhs_gt_A : A < phi (recΔ b s (delta n)) := by
>     have : A < A + omega0 * (phi b + 1) + 1 := by
>       have hpos : (0 : Ordinal) < omega0 * (phi b + 1) + 1 := by
>         have : (0 : Ordinal) < 1 := by norm_num
>         exact lt_of_le_of_lt (zero_le _) (lt_of_le_of_lt (le_of_eq (by rfl)) this)
>       have : A + omega0 * (phi b + 1) + 1 = A + (omega0 * (phi b + 1) + 1) := by simp [add_assoc]
>       simpa [this] using lt_add_of_pos_right A hpos
>     simpa [phi, hA]
>   have : phi (merge s (recΔ b s n)) < A := by
>     have eq_mu : phi (merge s (recΔ b s n)) = (omega0 ^ (3 : Ordinal)) * (phi s + 1)
>         + ((omega0 ^ (2 : Ordinal)) * (phi (recΔ b s n) + 1) + 1) := by simp [phi, add_assoc]
>     simpa [eq_mu] using sum_lt
>   exact lt_trans this rhs_gt_A





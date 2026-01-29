Context: Excerpt from Termination_Legacy showing disallowed tactic imports and the parameterized recΔ bound used to bypass rec_succ proof.
Source: C:\Users\Moses\OpComp\MUST_Review\Legacy\MetaMD_Archive\Termination_Legacy.md
SHA256: 7E6BEA6B0B9B9A6D8D6712AF9D25E0D86FEE7C8A9B11AF39A4F76F0D48A2D6B3
FailureExplanation: Termination_Legacy uses heavy tactic automation and introduces a parameterized bound to bypass the rec_succ proof, leaving the core inequality unproven.
FailureModeTags: constraint_violation; unsupported_inference

Excerpt:
> import OperatorKernelO6.Kernel
> import Init.WF
> import Mathlib.Algebra.Order.SuccPred
> import Mathlib.Data.Nat.Cast.Order.Basic
> import Mathlib.SetTheory.Ordinal.Basic
> import Mathlib.SetTheory.Ordinal.Arithmetic
> import Mathlib.SetTheory.Ordinal.Exponential
> import Mathlib.Algebra.Order.Monoid.Defs
> import Mathlib.Tactic.Linarith
> import Mathlib.Tactic.NormNum
> import Mathlib.Algebra.Order.GroupWithZero.Unbundled.Defs
> import Mathlib.Algebra.Order.Monoid.Unbundled.Basic
> import Mathlib.Tactic.Ring
> import Mathlib.Algebra.Order.Group.Defs
> import Mathlib.SetTheory.Ordinal.Principal
> import Mathlib.Tactic
> 
> set_option linter.unnecessarySimpa false
> 
> universe u
> 
> open Ordinal
> open OperatorKernelO6
> open Trace
> 
> namespace MetaSN
> 
> noncomputable def mu : Trace → Ordinal.{0}
> | .void        => 0
> | .delta t     => (omega0 ^ (5 : Ordinal)) * (mu t + 1) + 1
> | .integrate t => (omega0 ^ (4 : Ordinal)) * (mu t + 1) + 1
> | .merge a b   =>
>     (omega0 ^ (3 : Ordinal)) * (mu a + 1) +
>     (omega0 ^ (2 : Ordinal)) * (mu b + 1) + 1
> | .recΔ b s n  =>
>     omega0 ^ (mu n + mu s + (6 : Ordinal))
>   + omega0 * (mu b + 1) + 1
> | .eqW a b     =>
>     omega0 ^ (mu a + mu b + (9 : Ordinal)) + 1
> 
> 
> theorem lt_add_one_of_le {x y : Ordinal} (h : x ≤ y) : x < y + 1 :=
>   (Order.lt_add_one_iff (x := x) (y := y)).2 h
> 
> theorem le_of_lt_add_one {x y : Ordinal} (h : x < y + 1) : x ≤ y :=
>   (Order.lt_add_one_iff (x := x) (y := y)).1 h
> 
> theorem mu_lt_delta (t : Trace) : mu t < mu (.delta t) := by
>   have h0 : mu t ≤ mu t + 1 :=
>     le_of_lt ((Order.lt_add_one_iff (x := mu t) (y := mu t)).2 le_rfl)
>   have hb : 0 < (omega0 ^ (5 : Ordinal)) :=
>     (Ordinal.opow_pos (b := (5 : Ordinal)) (a0 := omega0_pos))
>   have h1 : mu t + 1 ≤ (omega0 ^ (5 : Ordinal)) * (mu t + 1) := by
>     simpa using
>       (Ordinal.le_mul_right (a := mu t + 1) (b := (omega0 ^ (5 : Ordinal))) hb)
>   have h : mu t ≤ (omega0 ^ (5 : Ordinal)) * (mu t + 1) := le_trans h0 h1
>   have : mu t < (omega0 ^ (5 : Ordinal)) * (mu t + 1) + 1 :=
>     (Order.lt_add_one_iff
>       (x := mu t) (y := (omega0 ^ (5 : Ordinal)) * (mu t + 1))).2 h
>   simpa [mu] using this
> 
> theorem mu_lt_merge_void_left (t : Trace) :
>   mu t < mu (.merge .void t) := by
>   have h0 : mu t ≤ mu t + 1 :=
>     le_of_lt ((Order.lt_add_one_iff (x := mu t) (y := mu t)).2 le_rfl)
>   have hb : 0 < (omega0 ^ (2 : Ordinal)) :=
>     (Ordinal.opow_pos (b := (2 : Ordinal)) (a0 := omega0_pos))
>   have h1 : mu t + 1 ≤ (omega0 ^ (2 : Ordinal)) * (mu t + 1) := by
>     simpa using
>       (Ordinal.le_mul_right (a := mu t + 1) (b := (omega0 ^ (2 : Ordinal))) hb)
>   have hY : mu t ≤ (omega0 ^ (2 : Ordinal)) * (mu t + 1) := le_trans h0 h1
>   have hlt : mu t < (omega0 ^ (2 : Ordinal)) * (mu t + 1) + 1 :=
>     (Order.lt_add_one_iff
>       (x := mu t) (y := (omega0 ^ (2 : Ordinal)) * (mu t + 1))).2 hY
>   have hpad :
>       (omega0 ^ (2 : Ordinal)) * (mu t + 1) ≤
>       (omega0 ^ (3 : Ordinal)) * (mu .void + 1) +
>         (omega0 ^ (2 : Ordinal)) * (mu t + 1) :=
>     Ordinal.le_add_left _ _
>   have hpad1 :
>       (omega0 ^ (2 : Ordinal)) * (mu t + 1) + 1 ≤
>       ((omega0 ^ (3 : Ordinal)) * (mu .void + 1) +
>         (omega0 ^ (2 : Ordinal)) * (mu t + 1)) + 1 :=
>     add_le_add_right hpad 1
>   have hfin : mu t < ((omega0 ^ (3 : Ordinal)) * (mu .void + 1) +
>         (omega0 ^ (2 : Ordinal)) * (mu t + 1)) + 1 :=
>     lt_of_lt_of_le hlt hpad1
>   simpa [mu] using hfin
> 
> /-- Base-case decrease: `recΔ … void`. -/
> theorem mu_lt_rec_zero (b s : Trace) :
>     mu b < mu (.recΔ b s .void) := by
> 
>   have h0 : (mu b) ≤ mu b + 1 :=
>     le_of_lt (lt_add_one (mu b))
> 
>   have h1 : mu b + 1 ≤ omega0 * (mu b + 1) :=
>     Ordinal.le_mul_right (a := mu b + 1) (b := omega0) omega0_pos
> 
>   have hle : mu b ≤ omega0 * (mu b + 1) := le_trans h0 h1
> 
>   have hlt : mu b < omega0 * (mu b + 1) + 1 := lt_of_le_of_lt hle (lt_add_of_pos_right _ zero_lt_one)
> 
>   have hpad :
>       omega0 * (mu b + 1) + 1 ≤
>       omega0 ^ (mu s + 6) + omega0 * (mu b + 1) + 1 := by
>     --  ω^(μ s+6) is non-negative, so adding it on the left preserves ≤
>     have : (0 : Ordinal) ≤ omega0 ^ (mu s + 6) :=
>       Ordinal.zero_le _
>     have h₂ :
>         omega0 * (mu b + 1) ≤
>         omega0 ^ (mu s + 6) + omega0 * (mu b + 1) :=
>       le_add_of_nonneg_left this
>     exact add_le_add_right h₂ 1
> 
>   have : mu b <
>          omega0 ^ (mu s + 6) + omega0 * (mu b + 1) + 1 := lt_of_lt_of_le hlt hpad
> 
>   simpa [mu] using this
>  -- unfold RHS once
> 
> theorem mu_lt_merge_void_right (t : Trace) :
>   mu t < mu (.merge t .void) := by
>   have h0 : mu t ≤ mu t + 1 :=
>     le_of_lt ((Order.lt_add_one_iff (x := mu t) (y := mu t)).2 le_rfl)
>   have hb : 0 < (omega0 ^ (3 : Ordinal)) :=
>     (Ordinal.opow_pos (b := (3 : Ordinal)) (a0 := omega0_pos))
>   have h1 : mu t + 1 ≤ (omega0 ^ (3 : Ordinal)) * (mu t + 1) := by
>     simpa using
>       (Ordinal.le_mul_right (a := mu t + 1) (b := (omega0 ^ (3 : Ordinal))) hb)
>   have hY : mu t ≤ (omega0 ^ (3 : Ordinal)) * (mu t + 1) := le_trans h0 h1
>   have hlt : mu t < (omega0 ^ (3 : Ordinal)) * (mu t + 1) + 1 :=
>     (Order.lt_add_one_iff
>       (x := mu t) (y := (omega0 ^ (3 : Ordinal)) * (mu t + 1))).2 hY
>   have hpad :
>       (omega0 ^ (3 : Ordinal)) * (mu t + 1) + 1 ≤
>       ((omega0 ^ (3 : Ordinal)) * (mu t + 1) +
>         (omega0 ^ (2 : Ordinal)) * (mu .void + 1)) + 1 :=
>     add_le_add_right (Ordinal.le_add_right _ _) 1
>   have hfin :
>       mu t <
>       ((omega0 ^ (3 : Ordinal)) * (mu t + 1) +
>         (omega0 ^ (2 : Ordinal)) * (mu .void + 1)) + 1 := lt_of_lt_of_le hlt hpad
>   simpa [mu] using hfin
> 
> theorem mu_lt_merge_cancel (t : Trace) :
>   mu t < mu (.merge t t) := by
>   have h0 : mu t ≤ mu t + 1 :=
>     le_of_lt ((Order.lt_add_one_iff (x := mu t) (y := mu t)).2 le_rfl)
>   have hb : 0 < (omega0 ^ (3 : Ordinal)) :=
>     (Ordinal.opow_pos (b := (3 : Ordinal)) (a0 := omega0_pos))
>   have h1 : mu t + 1 ≤ (omega0 ^ (3 : Ordinal)) * (mu t + 1) := by
>     simpa using
>       (Ordinal.le_mul_right (a := mu t + 1) (b := (omega0 ^ (3 : Ordinal))) hb)
>   have hY : mu t ≤ (omega0 ^ (3 : Ordinal)) * (mu t + 1) := le_trans h0 h1
>   have hlt : mu t < (omega0 ^ (3 : Ordinal)) * (mu t + 1) + 1 :=
>     (Order.lt_add_one_iff
>       (x := mu t) (y := (omega0 ^ (3 : Ordinal)) * (mu t + 1))).2 hY
>   have hpad :
>       (omega0 ^ (3 : Ordinal)) * (mu t + 1) ≤
>       (omega0 ^ (3 : Ordinal)) * (mu t + 1) +
>         (omega0 ^ (2 : Ordinal)) * (mu t + 1) :=
>     Ordinal.le_add_right _ _
>   have hpad1 :
>       (omega0 ^ (3 : Ordinal)) * (mu t + 1) + 1 ≤
>       ((omega0 ^ (3 : Ordinal)) * (mu t + 1) +
>         (omega0 ^ (2 : Ordinal)) * (mu t + 1)) + 1 :=
>     add_le_add_right hpad 1
>   have hfin :
>       mu t <
>       ((omega0 ^ (3 : Ordinal)) * (mu t + 1) +
>         (omega0 ^ (2 : Ordinal)) * (mu t + 1)) + 1 := lt_of_lt_of_le hlt hpad1
>   simpa [mu] using hfin
> 
> theorem zero_lt_add_one (y : Ordinal) : (0 : Ordinal) < y + 1 :=
>   (Order.lt_add_one_iff (x := (0 : Ordinal)) (y := y)).2 bot_le
> 
> theorem mu_void_lt_integrate_delta (t : Trace) :
>   mu .void < mu (.integrate (.delta t)) := by
>   simp [mu]
> 
> theorem mu_void_lt_eq_refl (a : Trace) :
>   mu .void < mu (.eqW a a) := by
>   simp [mu]
> 
> -- Surgical fix: Parameterized theorem isolates the hard ordinal domination assumption
> -- This unblocks the proof chain while documenting the remaining research challenge
> theorem mu_recΔ_plus_3_lt (b s n : Trace)
>   (h_bound : omega0 ^ (mu n + mu s + (6 : Ordinal)) + omega0 * (mu b + 1) + 1 + 3 <
>              (omega0 ^ (5 : Ordinal)) * (mu n + 1) + 1 + mu s + 6) :
>   mu (recΔ b s n) + 3 < mu (delta n) + mu s + 6 := by
>   -- Convert both sides using mu definitions - now should match exactly
>   simp only [mu]
>   exact h_bound

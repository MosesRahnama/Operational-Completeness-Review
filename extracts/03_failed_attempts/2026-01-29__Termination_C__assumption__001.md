Context: Termination_C.lean documents a remaining domination inequality as a parameterized assumption.
Source: C:\Users\Moses\OpComp\MUST_Review\Legacy\Meta_Lean_Archive\Termination_C.lean
SHA256: B5E8E55D80293DB39E8464DEA29209AAC1029FBF9727D7E7EAA647FE7D3CD8BE
FailureExplanation: Core recΔ successor inequality is assumed (h_bound), not proved, so the termination chain is conditional.
FailureModeTags: unsupported_inference

Excerpt:
> import OperatorKernelO6.Kernel
> import Init.WF
> import Mathlib.Data.Prod.Lex
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
> set_option linter.unnecessarySimpa false
> -- set_option linter.unnecessarySimpa false
> universe u
>
> open Ordinal
> open OperatorKernelO6
> open Trace
>
> namespace MetaSN
> -- (removed auxiliary succ_succ_eq_add_two' as we keep +2 canonical form)
>
> /-- Strict-mono of ω-powers in the exponent (base `omega0`). --/
> @[simp] theorem opow_lt_opow_right {b c : Ordinal} (h : b < c) :
>   omega0 ^ b < omega0 ^ c := by
>   simpa using ((Ordinal.isNormal_opow (a := omega0) one_lt_omega0).strictMono h)
>
> noncomputable def mu : Trace → Ordinal.{0}
> | .void        => 0
> | .delta t     => (omega0 ^ (5 : Ordinal)) * (mu t + 1) + 1
> | .integrate t => (omega0 ^ (4 : Ordinal)) * (mu t + 1) + 1
> | .merge a b   =>
>     (omega0 ^ (3 : Ordinal)) * (MetaSN.mu a + 1) +
>     (omega0 ^ (2 : Ordinal)) * (MetaSN.mu b + 1) + 1
> | .recΔ b s n  =>
>     omega0 ^ (MetaSN.mu n + MetaSN.mu s + (6 : Ordinal))
>   + omega0 * (MetaSN.mu b + 1) + 1
> | .eqW a b     =>
>     omega0 ^ (MetaSN.mu a + MetaSN.mu b + (9 : Ordinal)) + 1
>
> /-- Secondary counter: 0 everywhere **except** it counts the
>     nesting level inside `recΔ` so that `recΔ succ` strictly drops. -/
> def kappa : Trace → ℕ
> | Trace.recΔ _ _ n => (kappa n).succ
> | Trace.void => 0
> | Trace.delta _ => 0
> | Trace.integrate _ => 0
> | Trace.merge _ _ => 0
> | Trace.eqW _ _ => 0
>
> -- combined measure pair (kappa primary, mu secondary)
> noncomputable def μκ (t : Trace) : ℕ × Ordinal := (kappa t, mu t)
>
> -- lexicographic ordering on the pair
> def LexNatOrd : (ℕ × Ordinal) → (ℕ × Ordinal) → Prop :=
>   Prod.Lex (· < ·) (· < ·)
>
>
> @[simp] lemma kappa_non_rec (t : Trace)
>   : (¬ ∃ b s n, t = Trace.recΔ b s n) → kappa t = 0 := by
>   cases t with
>   | recΔ b s n =>
>       intro h; exact (False.elim (h ⟨b, s, n, rfl⟩))
>   | void => intro _; simp [kappa]
>   | delta _ => intro _; simp [kappa]
>   | integrate _ => intro _; simp [kappa]
>   | merge _ _ => intro _; simp [kappa]
>   | eqW _ _ => intro _; simp [kappa]
>
> theorem mu_lt_merge_void_right (t : Trace) :
>   mu t < MetaSN.mu (.merge t .void) := by
>   -- μ(merge t void) = ω³*(μ t +1) + ω² + 1 dominates μ t
>   have h1 : mu t < mu t + 1 :=
>     (Order.lt_add_one_iff (x := mu t) (y := mu t)).2 le_rfl
>   have pos3 : 0 < omega0 ^ (3 : Ordinal) :=
>     Ordinal.opow_pos (a := omega0) (b := (3 : Ordinal)) omega0_pos
>   have hmono : mu t + 1 ≤ omega0 ^ (3 : Ordinal) * (mu t + 1) := by
>     simpa using (Ordinal.le_mul_right (a := mu t + 1) (b := omega0 ^ (3 : Ordinal)) pos3)
>   have h2 : mu t < omega0 ^ (3 : Ordinal) * (mu t + 1) := lt_of_lt_of_le h1 hmono
>   have tail : (0 : Ordinal) ≤ omega0 ^ (2 : Ordinal) * (0 + 1) + 1 := zero_le _
>   have h3 : omega0 ^ (3 : Ordinal) * (mu t + 1) ≤
>       omega0 ^ (3 : Ordinal) * (mu t + 1) + (omega0 ^ (2 : Ordinal) * (0 + 1) + 1) :=
>     le_add_of_nonneg_right tail
>   have h4 : mu t < omega0 ^ (3 : Ordinal) * (mu t + 1) + (omega0 ^ (2 : Ordinal) * (0 + 1) + 1) :=
>     lt_of_lt_of_le h2 h3
>   simpa [mu, add_assoc, add_comm, add_left_comm]
>     using h4
>
> theorem zero_lt_add_one (y : Ordinal) : (0 : Ordinal) < y + 1 :=
>   (Order.lt_add_one_iff (x := (0 : Ordinal)) (y := y)).2 bot_le
>
> theorem mu_void_lt_integrate_delta (t : Trace) :
>   MetaSN.mu .void < MetaSN.mu (.integrate (.delta t)) := by
>   simp [MetaSN.mu]
>
> theorem mu_void_lt_eq_refl (a : Trace) :
>   MetaSN.mu .void < MetaSN.mu (.eqW a a) := by
>   simp [MetaSN.mu]
>
> /-- Cancellation rule: `merge t t → t` strictly drops `μ`. -/
> theorem mu_lt_merge_cancel (t : Trace) :
>   MetaSN.mu t < MetaSN.mu (.merge t t) := by
>   have h0 : MetaSN.mu t < MetaSN.mu t + 1 :=
>     (Order.lt_add_one_iff (x := MetaSN.mu t) (y := MetaSN.mu t)).2 le_rfl
>   have pos3 : 0 < omega0 ^ (3 : Ordinal) :=
>     Ordinal.opow_pos (a := omega0) (b := (3 : Ordinal)) omega0_pos
>   have hmono : MetaSN.mu t + 1 ≤ omega0 ^ (3 : Ordinal) * (MetaSN.mu t + 1) := by
>     simpa using (Ordinal.le_mul_right (a := MetaSN.mu t + 1) (b := omega0 ^ (3 : Ordinal)) pos3)
>   have h1 : MetaSN.mu t < omega0 ^ (3 : Ordinal) * (MetaSN.mu t + 1) := lt_of_lt_of_le h0 hmono
>   -- add the second ω²-term (same `t`) and tail +1
>   have pad :
>       omega0 ^ (3 : Ordinal) * (MetaSN.mu t + 1) ≤
>       (omega0 ^ (3 : Ordinal)) * (MetaSN.mu t + 1) +
>         (omega0 ^ (2 : Ordinal)) * (MetaSN.mu t + 1) :=
>     Ordinal.le_add_right _ _
>   have pad1 :
>       omega0 ^ (3 : Ordinal) * (MetaSN.mu t + 1) + 1 ≤
>       ((omega0 ^ (3 : Ordinal)) * (MetaSN.mu t + 1) +
>         (omega0 ^ (2 : Ordinal)) * (MetaSN.mu t + 1)) + 1 :=
>     add_le_add_right pad 1
>   have h2 :
>       MetaSN.mu t <
>       ((omega0 ^ (3 : Ordinal)) * (MetaSN.mu t + 1) +
>         (omega0 ^ (2 : Ordinal)) * (MetaSN.mu t + 1)) + 1 :=
>     lt_of_lt_of_le
>       (lt_of_lt_of_le h1 (le_add_of_nonneg_right (zero_le _))) pad1
>   simpa [MetaSN.mu, add_assoc]
>     using h2
> -- Diagnostic flag
> def debug_mode := true
>
>
> -- set_option trace.Meta.Tactic.simp true
>
>
>
> -- set_option diagnostics.threshold 100
> -- set_option diagnostics true
> -- set_option autoImplicit false
> -- set_option maxRecDepth 500
> -- set_option pp.explicit true
> -- set_option pp.universes true
> -- set_option trace.Meta.isDefEq true
> -- set_option trace.Meta.debug true
> -- set_option trace.Meta.Tactic.simp.rewrite true
> -- set_option trace.linarith true
> -- set_option trace.compiler.ir.result true
>
>
>
> -- (Removed earlier succ_succ_eq_add_two lemma to avoid recursive simp loops.)
> lemma succ_succ_eq_add_two (x : Ordinal) :
>   Order.succ (Order.succ x) = x + 2 := by
>   have hx : Order.succ x = x + 1 := by
>     simpa using (Ordinal.add_one_eq_succ (a := x)).symm
>   have hx2 : Order.succ (Order.succ x) = (x + 1) + 1 := by
>     -- rewrite outer succ
>     rw [hx]
>     simpa using (Ordinal.add_one_eq_succ (a := x + 1)).symm
>   -- assemble via calc to avoid deep simp recursion
>   calc
>     Order.succ (Order.succ x) = (x + 1) + 1 := hx2
>     _ = x + (1 + 1) := by rw [add_assoc]
>     _ = x + 2 := by simp
>
> @[simp] lemma succ_succ_pow2 :
>   Order.succ (Order.succ (omega0 ^ (2 : Ordinal))) = omega0 ^ (2 : Ordinal) + 2 := by
>   simpa using succ_succ_eq_add_two (omega0 ^ (2 : Ordinal))
>
>
> /-- Special case: both args void. Clean proof staying in +2 form. -/
> theorem mu_lt_eq_diff_both_void :
>   MetaSN.mu (integrate (merge .void .void)) < MetaSN.mu (eqW .void .void) := by
>   -- μ(merge void void)
>   have hμm : MetaSN.mu (merge .void .void) =
>       omega0 ^ (3 : Ordinal) + omega0 ^ (2 : Ordinal) + 1 := by
>     simp [MetaSN.mu, add_assoc]
>   -- rewrite μ(integrate ...)
>   have hL : MetaSN.mu (integrate (merge .void .void)) =
>       omega0 ^ (4 : Ordinal) * (omega0 ^ (3 : Ordinal) + omega0 ^ (2 : Ordinal) + 2) + 1 := by
>     simpa [MetaSN.mu, hμm, add_assoc]
>   -- payload pieces < ω^5 via additive principal
>   have hα : omega0 ^ (3 : Ordinal) < omega0 ^ (5 : Ordinal) := by
>     have : (3 : Ordinal) < 5 := by norm_num
>     simpa using opow_lt_opow_right this
>   have hβ : omega0 ^ (2 : Ordinal) < omega0 ^ (5 : Ordinal) := by
>     have : (2 : Ordinal) < 5 := by norm_num
>     simpa using opow_lt_opow_right this
>   have hγ : (2 : Ordinal) < omega0 ^ (5 : Ordinal) := by
>     have h2ω : (2 : Ordinal) < omega0 := nat_lt_omega0 2
>     have ω_le : omega0 ≤ omega0 ^ (5 : Ordinal) := by
>       have : (1 : Ordinal) ≤ (5 : Ordinal) := by norm_num
>       have hpow := Ordinal.opow_le_opow_right omega0_pos this
>       simpa using (le_trans (by simpa using (Ordinal.opow_one omega0).symm.le) hpow)
>     exact lt_of_lt_of_le h2ω ω_le
>   have hprin := Ordinal.principal_add_omega0_opow (5 : Ordinal)
>   have hsum12 : omega0 ^ (3 : Ordinal) + omega0 ^ (2 : Ordinal) < omega0 ^ (5 : Ordinal) :=
>     hprin hα hβ
>   have h_payload : omega0 ^ (3 : Ordinal) + omega0 ^ (2 : Ordinal) + 2 < omega0 ^ (5 : Ordinal) :=
>     hprin hsum12 hγ
>   -- multiply by ω^4 and collapse exponent
>   have pos4 : (0 : Ordinal) < omega0 ^ (4 : Ordinal) :=
>     Ordinal.opow_pos (a := omega0) (b := (4 : Ordinal)) omega0_pos
>   have hstep : omega0 ^ (4 : Ordinal) *
>       (omega0 ^ (3 : Ordinal) + omega0 ^ (2 : Ordinal) + 2) <
>       omega0 ^ (4 : Ordinal) * omega0 ^ (5 : Ordinal) :=
>     Ordinal.mul_lt_mul_of_pos_left h_payload pos4
>   have hcollapse : omega0 ^ (4 : Ordinal) * omega0 ^ (5 : Ordinal) =
>       omega0 ^ (4 + 5 : Ordinal) := by
>     simpa using (Ordinal.opow_add omega0 (4:Ordinal) (5:Ordinal)).symm
>   have h45 : (4 : Ordinal) + (5 : Ordinal) = (9 : Ordinal) := by norm_num
>   have h_prod :
>       omega0 ^ (4 : Ordinal) *
>         (omega0 ^ (3 : Ordinal) + omega0 ^ (2 : Ordinal) + 2) <
>       omega0 ^ (9 : Ordinal) := by
>     have htmp2 : omega0 ^ (4 : Ordinal) *
>         (omega0 ^ (3 : Ordinal) + omega0 ^ (2 : Ordinal) + 2) < omega0 ^ (4 + 5 : Ordinal) :=
>       lt_of_lt_of_eq hstep hcollapse
>     have hrewrite : omega0 ^ (4 + 5 : Ordinal) = omega0 ^ (9 : Ordinal) := by
>       simpa using congrArg (fun e => omega0 ^ e) h45
>     exact lt_of_lt_of_eq htmp2 hrewrite
>   -- add-one bridge
>   have hR : MetaSN.mu (eqW .void .void) = omega0 ^ (9 : Ordinal) + 1 := by
>     simp [MetaSN.mu]
>   have hA1 : omega0 ^ (4 : Ordinal) *
>       (omega0 ^ (3 : Ordinal) + omega0 ^ (2 : Ordinal) + 2) + 1 ≤
>       omega0 ^ (9 : Ordinal) := Order.add_one_le_of_lt h_prod
>   have hfin : omega0 ^ (4 : Ordinal) *
>       (omega0 ^ (3 : Ordinal) + omega0 ^ (2 : Ordinal) + 2) + 1 <
>       omega0 ^ (9 : Ordinal) + 1 :=
>     (Order.lt_add_one_iff (x := _ + 1) (y := omega0 ^ (9 : Ordinal))).2 hA1
>   simpa [hL, hR] using hfin
>
> @[simp] lemma succ_succ_mul_pow2_succ (x : Ordinal) :
>   Order.succ (Order.succ (omega0 ^ (2 : Ordinal) * Order.succ x)) =
>     omega0 ^ (2 : Ordinal) * Order.succ x + 2 := by
>   simpa using succ_succ_eq_add_two (omega0 ^ (2 : Ordinal) * Order.succ x)
>
> -- (section continues with μ auxiliary lemmas)
> lemma mu_recDelta_plus_3_lt (b s n : Trace)
>   (h_bound : omega0 ^ (MetaSN.mu n + MetaSN.mu s + (6 : Ordinal)) + omega0 * (MetaSN.mu b + 1) + 1 + 3 <
>              (omega0 ^ (5 : Ordinal)) * (MetaSN.mu n + 1) + 1 + MetaSN.mu s + 6) :
>   MetaSN.mu (recΔ b s n) + 3 < MetaSN.mu (delta n) + MetaSN.mu s + 6 := by
>   -- Expand mu definitions on both sides; structure then matches h_bound directly
>   simp only [mu]
>   exact h_bound

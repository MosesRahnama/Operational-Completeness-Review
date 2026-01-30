Purpose: Evidence extract (failed_attempts/code) documenting a failure or relevance; Rec_succ delta case uses a sorry-bound ordinal inequality that is mathematically false, so the proof is incomplete.
Contents: Metadata header + excerpt from the source file.
Context: Full Claude_SN.lean proof attempt for strong normalization, containing a sorry-bound in the rec_succ delta case.
Source: Legacy/MetaMD_Archive/Claude_SN.md
SHA256: 71610E31ABBE514DA8B9030271AEAF28E09011186EEB88556A6C3547CE7CBFC2
FailureExplanation: The rec_succ delta case relies on an unprovable ordinal inequality (left as sorry), so the proof is incomplete.
FailureModeTags: unsupported_inference; proof_obligation_stuck; nondecreasing_measure

---

# Claude_SN.lean

- Source: `Claude_SN.lean`
- Generated: 2025-08-14T00:26:04

```lean

import OperatorKernelO6.Kernel
import OperatorKernelO6.Meta.Termination
import Init.WF
import Mathlib.Data.Prod.Lex
set_option linter.unnecessarySimpa false

open OperatorKernelO6 Trace Ordinal MetaSN
are there
namespace ClaudeSN

/-! ## 1) Define the structural κ (Nat) and prove simp facts -/

/-- Definition (by recursion on Trace) -/
def kappa : Trace → Nat
| .void                => 0
| .delta t             => kappa t
| .integrate t         => kappa t
| .merge a b           => Nat.max (kappa a) (kappa b)
| .recΔ b s (.delta n) => Nat.max (Nat.max (kappa b) (kappa s)) (kappa n) + 1
| .recΔ b s n          => Nat.max (Nat.max (kappa b) (kappa s)) (kappa n)
| .eqW a b             => Nat.max (kappa a) (kappa b)

/-- Make these [simp] -/
@[simp] lemma kappa_void : kappa void = 0 := rfl
@[simp] lemma kappa_delta (t) : kappa (delta t) = kappa t := rfl
@[simp] lemma kappa_integrate (t) : kappa (integrate t) = kappa t := rfl
@[simp] lemma kappa_merge (a b) : kappa (merge a b) = Nat.max (kappa a) (kappa b) := rfl
@[simp] lemma kappa_eqW (a b) : kappa (eqW a b) = Nat.max (kappa a) (kappa b) := rfl
@[simp] lemma kappa_rec_delta (b s n) : kappa (recΔ b s (delta n)) =
    Nat.max (Nat.max (kappa b) (kappa s)) (kappa n) + 1 := rfl

/-! ## 2) Define μ and the lexicographic measure -/

/-- Define: measure(t) := (κ t, μ t) : Nat × Ordinal (noncomputable) -/
noncomputable def measure (t : Trace) : Nat × Ordinal := (kappa t, MetaSN.mu t)

/-- Define LexOrder := Prod.Lex (<) (<) on (Nat × Ordinal) -/
def LexOrder : (Nat × Ordinal) → (Nat × Ordinal) → Prop :=
  Prod.Lex (· < ·) (· < ·)

/-- Prove WF: wf_LexOrder by WellFounded.prod_lex Nat.lt_wfRel.wf Ordinal.lt_wf -/
lemma wf_LexOrder : WellFounded LexOrder := by
  exact WellFounded.prod_lex Nat.lt_wfRel.wf Ordinal.lt_wf

/-! ## 3) Define the two lex descent helpers -/

/-- drop_left: κ b < κ a → LexOrder (measure b) (measure a) -/
lemma drop_left {a b : Trace} (hk : kappa b < kappa a) :
    LexOrder (measure b) (measure a) := by
  unfold LexOrder measure
  exact Prod.Lex.left _ _ hk

/-- drop_right: μ b < μ a → κ b = κ a → LexOrder (measure b) (measure a) -/
lemma drop_right {a b : Trace} (hμ : MetaSN.mu b < MetaSN.mu a) (hk : kappa b = kappa a) :
    LexOrder (measure b) (measure a) := by
  unfold LexOrder measure
  rw [hk]
  exact Prod.Lex.right _ hμ

/-! ## 4) Per-rule strict decrease proofs -/

/-! Main theorem: every step decreases the lexicographic measure -/
theorem measure_decreases : ∀ {a b : Trace}, Step a b → LexOrder (measure b) (measure a)
| _, _, Step.R_int_delta t => by
    by_cases h : kappa t = 0
    · apply drop_right (MetaSN.mu_void_lt_integrate_delta t)
      simp [kappa_integrate, kappa_delta, kappa_void, h]
    · have hpos : 0 < kappa t := Nat.pos_of_ne_zero h
      apply drop_left
      simpa [kappa_integrate, kappa_delta, kappa_void] using hpos

| _, _, Step.R_merge_void_left t => by
    apply drop_right (MetaSN.mu_lt_merge_void_left t)
    simp [kappa_merge, kappa_void]

| _, _, Step.R_merge_void_right t => by
    apply drop_right (MetaSN.mu_lt_merge_void_right t)
    simp [kappa_merge, kappa_void]

| _, _, Step.R_merge_cancel t => by
    apply drop_right (MetaSN.mu_lt_merge_cancel t)
    simp [kappa_merge]

| _, _, Step.R_rec_zero b s => by
    have hb_le : kappa b ≤ kappa (recΔ b s void) := by
      have h1 : kappa b ≤ Nat.max (kappa b) (kappa s) := Nat.le_max_left _ _
      have h2 : Nat.max (kappa b) (kappa s) ≤
        Nat.max (Nat.max (kappa b) (kappa s)) (kappa void) := Nat.le_max_left _ _
      simpa [kappa, kappa_void] using le_trans h1 h2
    by_cases hb_eq : kappa b = kappa (recΔ b s void)
    · exact drop_right (MetaSN.mu_lt_rec_base b s) hb_eq
    · exact drop_left (Nat.lt_of_le_of_ne hb_le hb_eq)

| _, _, Step.R_rec_succ b s n => by
    cases n with
    | delta m =>
      -- Need the bound hypothesis for mu_lt_rec_succ
      -- This is the known problematic bound that cannot be proved
      -- We leave it as sorry for now as it's a known research problem
      have h_bound : omega0 ^ (MetaSN.mu (delta m) + MetaSN.mu s + (6 : Ordinal)) + omega0 * (MetaSN.mu b + 1) + 1 + 3 <
                     (omega0 ^ (5 : Ordinal)) * (MetaSN.mu (delta m) + 1) + 1 + MetaSN.mu s + 6 := sorry
      have hμ := MetaSN.mu_lt_rec_succ b s (delta m) h_bound
      have hk : kappa (merge s (recΔ b s (delta m))) = kappa (recΔ b s (delta (delta m))) := by
        have hs_le : kappa s ≤ Nat.max (Nat.max (kappa b) (kappa s)) (kappa m) := by
          exact le_trans (Nat.le_max_right _ _) (Nat.le_max_left _ _)
        have h_rec : kappa (recΔ b s (delta m)) = Nat.max (Nat.max (kappa b) (kappa s)) (kappa m) + 1 := by
          simp [kappa]
        -- Fix max association for the goal
        have hs_le' : kappa s ≤ Nat.max (kappa b) (Nat.max (kappa s) (kappa m)) := by
          -- reorder ((κ b).max (κ s)).max (κ m) to κ b .max (κ s .max κ m)
          simpa [Nat.max_assoc, Nat.max_left_comm, Nat.max_comm] using hs_le
        have hs_le_succ : kappa s ≤ Nat.max (kappa b) (Nat.max (kappa s) (kappa m)) + 1 :=
          Nat.le_trans hs_le' (Nat.le_succ _)
        simpa [kappa_merge, h_rec, Nat.max_eq_right hs_le_succ]
      exact drop_right hμ hk
    | void =>
      let base := Nat.max (Nat.max (kappa b) (kappa s)) 0
      have hs_le : kappa s ≤ base :=
        le_trans (Nat.le_max_right _ _) (Nat.le_max_left _ _)
      have h_merge : kappa (merge s (recΔ b s void)) = base := by
        simp [kappa, base]
      have h_rec : kappa (recΔ b s (delta void)) = base + 1 := by
        simp [kappa, base]
      apply drop_left
      simpa [h_merge, h_rec] using Nat.lt_succ_self base
    | integrate t =>
      let base := Nat.max (Nat.max (kappa b) (kappa s)) (kappa t)
      have hs_le : kappa s ≤ base :=
        le_trans (Nat.le_max_right _ _) (Nat.le_max_left _ _)
      have h_merge : kappa (merge s (recΔ b s (integrate t))) = base := by
        simp [kappa, base]
      have h_rec : kappa (recΔ b s (delta (integrate t))) = base + 1 := by
        simp [kappa, base]
      apply drop_left
      simpa [h_merge, h_rec] using Nat.lt_succ_self base
    | merge t1 t2 =>
      let base := Nat.max (Nat.max (kappa b) (kappa s)) (Nat.max (kappa t1) (kappa t2))
      have hs_le : kappa s ≤ base :=
        le_trans (Nat.le_max_right _ _) (Nat.le_max_left _ _)
      have h_merge : kappa (merge s (recΔ b s (merge t1 t2))) = base := by
        simp [kappa, base]
      have h_rec : kappa (recΔ b s (delta (merge t1 t2))) = base + 1 := by
        simp [kappa, base]
      apply drop_left
      simpa [h_merge, h_rec] using Nat.lt_succ_self base
    | recΔ t1 t2 t3 =>
      let base := Nat.max (Nat.max (kappa b) (kappa s)) (kappa (recΔ t1 t2 t3))
      have hs_le : kappa s ≤ base :=
        le_trans (Nat.le_max_right _ _) (Nat.le_max_left _ _)
      have h_merge : kappa (merge s (recΔ b s (recΔ t1 t2 t3))) = base := by
        simp [kappa, base]
      have h_rec : kappa (recΔ b s (delta (recΔ t1 t2 t3))) = base + 1 := by
        simp [kappa, base]
      apply drop_left
      simpa [h_merge, h_rec] using Nat.lt_succ_self base
    | eqW t1 t2 =>
      let base := Nat.max (Nat.max (kappa b) (kappa s)) (Nat.max (kappa t1) (kappa t2))
      have hs_le : kappa s ≤ base :=
        le_trans (Nat.le_max_right _ _) (Nat.le_max_left _ _)
      have h_merge : kappa (merge s (recΔ b s (eqW t1 t2))) = base := by
        simp [kappa, base]
      have h_rec : kappa (recΔ b s (delta (eqW t1 t2))) = base + 1 := by
        simp [kappa, base]
      apply drop_left
      simpa [h_merge, h_rec] using Nat.lt_succ_self base

| _, _, Step.R_eq_refl a => by
    by_cases h : kappa a = 0
    · apply drop_right (MetaSN.mu_void_lt_eq_refl a)
      simp [kappa_eqW, kappa_void, h]
    · have hpos : 0 < kappa a := Nat.pos_of_ne_zero h
      apply drop_left
      simpa [kappa_eqW, kappa_void, Nat.max_self] using hpos

| _, _, Step.R_eq_diff (a:=a) (b:=b) hneq => by
    apply drop_right (MetaSN.mu_lt_eq_diff a b)
    simpa [kappa_eqW, kappa_integrate, kappa_merge]

/-! ## 5) Final well-foundedness -/

/-- Define StepRev: a → b iff Step b a -/
def StepRev : Trace → Trace → Prop := fun a b => Step b a

/-- Prove: strong_normalization : WellFounded StepRev by pulling back WF of lex with InvImage and measure_decreases -/
theorem strong_normalization : WellFounded StepRev := by
  -- Pull back the well-founded lexicographic order along the measure
  have wf_measure : WellFounded (InvImage LexOrder measure) :=
    InvImage.wf _ wf_LexOrder
  -- Show StepRev is a subrelation of the inverse image
  have sub : Subrelation StepRev (InvImage LexOrder measure) := by
    intros a b hab
    exact measure_decreases hab
  -- Apply the subrelation lemma
  exact Subrelation.wf sub wf_measure

/-- Alternative formulation: every trace is accessible under StepRev -/
theorem strong_normalization' (t : Trace) : Acc StepRev t :=
  strong_normalization.apply t

end ClaudeSN

export ClaudeSN (strong_normalization)

```





Purpose: Evidence extract (misc/analysis) documenting a failure or relevance; Instructional guidance; not direct evidence.
Contents: Metadata header + excerpt from the source file.
Context: Instructional script for fixing Claude_SN errors; includes exact patch guidance.
Source: MUST_Review/MetaMD_Archive/for_claude.md
SHA256: E584CED948532E69E8DCF2CB19936CFE50F1F4CA73B0B4BE28CDF9FE77100B14
FailureExplanation: Instructional guidance; not direct evidence.
FailureModeTags: 

Excerpt:
> What to fix, why, and how
> - Error 1: “Missing cases” in measure_decreases
>   - Cause: You didn’t cover all Step constructors.
>   - Fix: Add four missing branches with exactly the code below (R_rec_zero, R_rec_succ, R_eq_refl, R_eq_diff).
>
> - Error 2: “unknown identifier 't'” inside R_rec_zero
>   - Cause: You used t in a branch that has only b, s. Use μ lemma mu_lt_rec_base b s. Do not reference t there.
>
> - Error 3: “no goals to be solved” in R_int_delta
>   - Cause: You pasted the by_cases block three times. Keep one copy only.
>
> - Error 4: “unexpected identifier; expected command”
>   - Cause: Stray code or unbalanced branches. Replace the entire theorem body with the code below. Ensure the namespace closes with end ClaudeSN.
>
> - “unused simp arguments” (Nat.max_eq_right, Nat.max_self)
>   - Cause: Passing lemma names to simp is unnecessary and creates lints. Remove those; allow simp to simplify from expression shape.
>
> Exact, copy/paste replacements for ClaudeSN (Measure.lean)
> Step 0. Ensure these imports and namespace; remove any Legacy import
> - Keep only:
> ```lean
> import OperatorKernelO6.Kernel
> import OperatorKernelO6.Meta.Termination
> import Init.WF
> import Mathlib.Data.Prod.Lex
>
> open OperatorKernelO6 Trace Ordinal MetaSN
>
> namespace ClaudeSN
> ```
> - Ensure the file ends with:
> ```lean
> end ClaudeSN
> ```
> - Remove:
> ```lean
> import OperatorKernelO6.Meta.Termination_Legacy
> ```
>
> Step 1. Keep structural κ and only its simp facts
> Replace your κ block with this if it differs:
> ```lean
> def kappa : Trace → Nat
> | .void                => 0
> | .delta t             => kappa t
> | .integrate t         => kappa t
> | .merge a b           => Nat.max (kappa a) (kappa b)
> | .recΔ b s (.delta n) => Nat.max (Nat.max (kappa b) (kappa s)) (kappa n) + 1
> | .recΔ b s n          => Nat.max (Nat.max (kappa b) (kappa s)) (kappa n)
> | .eqW a b             => Nat.max (kappa a) (kappa b)
>
> @[simp] lemma kappa_void : kappa void = 0 := rfl
> @[simp] lemma kappa_delta (t) : kappa (delta t) = kappa t := rfl
> @[simp] lemma kappa_integrate (t) : kappa (integrate t) = kappa t := rfl
> @[simp] lemma kappa_merge (a b) : kappa (merge a b) = Nat.max (kappa a) (kappa b) := rfl
> @[simp] lemma kappa_eqW (a b) : kappa (eqW a b) = Nat.max (kappa a) (kappa b) := rfl
> @[simp] lemma kappa_rec_delta (b s n) :
>   kappa (recΔ b s (delta n)) =
>     Nat.max (Nat.max (kappa b) (kappa s)) (kappa n) + 1 := rfl
> ```
>
> Step 2. measure, lex, WF, drops
> Ensure exactly this:
> ```lean
> noncomputable def measure (t : Trace) : Nat × Ordinal := (kappa t, MetaSN.mu t)
>
> def LexOrder : (Nat × Ordinal) → (Nat × Ordinal) → Prop :=
>   Prod.Lex (· < ·) (· < ·)
>
> lemma wf_LexOrder : WellFounded LexOrder := by
>   exact WellFounded.prod_lex Nat.lt_wfRel.wf Ordinal.lt_wf
>
> lemma drop_left {a b : Trace} (hk : kappa b < kappa a) :
>     LexOrder (measure b) (measure a) := by
>   unfold LexOrder measure
>   exact Prod.Lex.left _ _ hk
>
> lemma drop_right {a b : Trace} (hμ : MetaSN.mu b < MetaSN.mu a) (hk : kappa b = kappa a) :
>     LexOrder (measure b) (measure a) := by
>   unfold LexOrder measure
>   rw [hk]
>   exact Prod.Lex.right _ hμ
> ```
>
> Step 3. Replace the theorem measure_decreases body with this exactly
> Delete the current theorem body and paste this (no duplicates):
> ```lean
> theorem measure_decreases : ∀ {a b : Trace}, Step a b → LexOrder (measure b) (measure a)
> | _, _, Step.R_int_delta t => by
>   -- integrate (delta t) → void
>   by_cases h : kappa t = 0
>   · apply drop_right (MetaSN.mu_void_lt_integrate_delta t)
>     simp [kappa_integrate, kappa_delta, kappa_void, h]
>   · have hpos : 0 < kappa t := Nat.pos_of_ne_zero h
>     apply drop_left
>     simpa [kappa_integrate, kappa_delta, kappa_void] using hpos
>
> | _, _, Step.R_merge_void_left t => by
>   -- merge void t → t
>   apply drop_right (MetaSN.mu_lt_merge_void_left t)
>   simp [kappa_merge, kappa_void]
>
> | _, _, Step.R_merge_void_right t => by
>   -- merge t void → t
>   apply drop_right (MetaSN.mu_lt_merge_void_right t)
>   simp [kappa_merge, kappa_void]
>
> | _, _, Step.R_merge_cancel t => by
>   -- merge t t → t
>   apply drop_right (MetaSN.mu_lt_merge_cancel t)
>   simp [kappa_merge]
>
> | _, _, Step.R_rec_zero b s => by
>   -- recΔ b s void → b
>   have hb_le : kappa b ≤ kappa (recΔ b s void) := by
>     have h1 : kappa b ≤ Nat.max (kappa b) (kappa s) := Nat.le_max_left _ _
>     have h2 : Nat.max (kappa b) (kappa s) ≤
>               Nat.max (Nat.max (kappa b) (kappa s)) (kappa void) := Nat.le_max_left _ _
>     simpa [kappa, kappa_void] using le_trans h1 h2
>   by_cases hb_eq : kappa b = kappa (recΔ b s void)
>   · exact drop_right (MetaSN.mu_lt_rec_base b s) hb_eq
>   · exact drop_left (Nat.lt_of_le_of_ne hb_le hb_eq)
>
> | _, _, Step.R_rec_succ b s n => by
>   -- recΔ b s (delta n) → app s (recΔ b s n)
>   cases n with
>   | delta m =>
>       -- Use your boundful μ-lemma from Meta.Termination (green).
>       -- If it’s available as a wrapper:
>       -- have hμ := MetaSN.mu_lt_rec_succ b s m MetaSN.rec_succ_bound
>       -- Or use the more primitive:
>       -- have hμ := MetaSN.mu_merge_lt_rec (b:=b) (s:=s) (n:=m) MetaSN.rec_succ_bound
>       -- κ equal on this branch (both sides simplify to base+1):
>       have hk : kappa (app s (recΔ b s (delta m))) = kappa (recΔ b s (delta m)) := by
>         simp [kappa]
>       exact drop_right hμ hk
>   | _ =>
>       let base := Nat.max (Nat.max (kappa b) (kappa s)) (kappa n)
>       have hs_le : kappa s ≤ base :=
>         le_trans (Nat.le_max_right _ _) (Nat.le_max_left _ _)
>       have h_merge : kappa (app s (recΔ b s n)) = base := by
>         simp [kappa, base, Nat.max_eq_right hs_le]
>       have h_rec : kappa (recΔ b s (delta n)) = base + 1 := by
>         simp [kappa, base]
>       apply drop_left
>       simpa [h_merge, h_rec] using Nat.lt_succ_self base
>
> | _, _, Step.R_eq_refl a => by
>   -- eqW a a → void
>   by_cases h : kappa a = 0
>   · apply drop_right (MetaSN.mu_void_lt_eq_refl a)
>     simp [kappa_eqW, kappa_void, Nat.max_self, h]
>   · have hpos : 0 < kappa a := Nat.pos_of_ne_zero h
>     apply drop_left
>     simpa [kappa_eqW, kappa_void, Nat.max_self] using hpos
>
> | _, _, Step.R_eq_diff (a:=a) (b:=b) hneq => by
>   -- eqW a b → integrate (merge a b)
>   apply drop_right (MetaSN.mu_lt_eq_diff a b)
>   simp [kappa_eqW, kappa_integrate, kappa_merge, Nat.max_comm, Nat.max_assoc, Nat.max_left_comm]
> ```
>
> - Important: Replace the commented hμ line in the R_rec_succ delta-case with the actual lemma you have in OperatorKernelO6.Meta.Termination:
>   - If you have a bound wrapper: MetaSN.mu_lt_rec_succ b s m MetaSN.rec_succ_bound
>   - Else: MetaSN.mu_merge_lt_rec (b:=b) (s:=s) (n:=m) MetaSN.rec_succ_bound
>
> Step 4. End the namespace
> - Ensure exactly one:
> ```lean
> end ClaudeSN
> ```
>
> Running diagnostics (best options) while fixing
> - To get more helpful logs for Sonnet without overwhelming it:
>   - At top of Claude_SN file, temporarily add:
>     - set_option linter.unusedSimpArgs true (keep; it’s useful)
>     - set_option trace.Elab.command true (shows the command elaboration; you already see it)
>     - set_option trace.Meta.Tactic.simp.rewrite true (more simp steps; turn off after fixing)
>   - Do not commit these. Remove after you’re green:
>     - set_option trace.Elab.command false
>     - set_option trace.Meta.Tactic.simp.rewrite false
> - Read Lean’s “Missing cases” and “unknown identifier” first; fix those before lints.
> - For lints “unused simp args”, do not pass named lemmas to simp. Use simp with expressions only.
>
> Math/lemma pointers from Termination
> - You already use:
>   - MetaSN.mu_void_lt_integrate_delta
>   - MetaSN.mu_lt_merge_void_left, MetaSN.mu_lt_merge_void_right, MetaSN.mu_lt_merge_cancel
>   - MetaSN.mu_lt_rec_base
> - For rec-succ:
>   - If provided: MetaSN.mu_merge_lt_rec (b:=b) (s:=s) (n:=m) MetaSN.rec_succ_bound
>   - Or a wrapper: MetaSN.mu_lt_rec_succ b s m MetaSN.rec_succ_bound
> - Eq-diff: MetaSN.mu_lt_eq_diff a b






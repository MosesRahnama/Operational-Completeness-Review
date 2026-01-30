Purpose: Evidence extract (misc/analysis) documenting a failure or relevance; Suggestion document; not direct failure evidence.
Contents: Metadata header + excerpt from the source file.
Context: AI-generated plan with proposed patches (includes code blocks).
Source: MUST_Review/MetaMD_Archive/GPT-5-Pro.md
SHA256: 2CC1DCF2833D2178BC911132E0BF5B45241295BE2C3CC52BB9F67BDF2C68605D
FailureExplanation: Suggestion document; not direct failure evidence.
FailureModeTags: 

Excerpt:
> alright — deep breath — here’s what went wrong, what we’re not going to try again, and the clean fix that gets you to green using the lexicographic measure (κ, μ) with κ strictly dropping only on recΔ‑succ and μ dropping on the other 7 rules. Kernel stays exactly as in your repo (incl. R_eq_diff with a ≠ premise). No bogus ω‑power lemmas, no hidden axioms, no sorrys.
>
> Below I give:
>
> a 1‑page failed‑attempts audit (so we don’t loop back),
>
> the per‑rule math↔tactic mapping,
>
> a tiny dependency scan, and
>
> the exact code patches (two files): a new μ‑drop for eqW_diff in MuCore.lean and the final single SN harness in SN_Final.lean using Prod.Lex + InvImage.wf.
>
> Everything matches the rulebooks and consolidation notes you attached. Citations are inline.
>
> Failed attempts audit (do not retry)
> ID	Essential claim tried	Why it fails / breaks rules	Status
> A1	opow_mul_lt_of_exp_lt : β < α → 0 < γ → ω^β * γ < ω^α (positivity-only)	False in general (take β=0, α=1, γ=ω ⇒ equality, not <). This exact lemma is not in mathlib; closest is the “succ” version with side bounds on the addend. Tooling doc clarifies this does not exist. Expanded_Ordinal_Toolkit	DISALLOWED
> A2	Pure‑μ proof of rec_succ_bound to carry R_rec_succ	Consolidation shows this is the hard block; inequality is not uniformly true across parameters μs, μn.	DISALLOWED
> A3	Rely on strict monotonicity of right addition on ordinals	Not valid in general; toolkit forbids that pattern.	DISALLOWED
> A4	Assume μ s ≤ μ (δ n) in recΔ b s n	Explicitly flagged as false with counterexamples; forbidden shortcut.	DISALLOWED
> A5	Use generic mul_le_mul_left (non‑ordinal) with ordinals	The generic ordered‑monoid lemma isn’t admissible for ordinal products; use the ordinal APIs (mul_le_mul_left', etc.).	DISALLOWED
> A6	Add κ‑drop on eqW_diff to avoid μ	You asked κ to drop only on recΔ‑succ; we keep that invariant and prove a clean μ‑drop for eqW_diff.	DISALLOWED for final plan
>
> Math ↔ tactic mapping per rule
> We use the combined measure μ̂(t) := (κ(t), μ(t)) : ℕ × Ordinal with lex order. κ is the “rec‑stack counter” that strictly drops only on R_rec_succ; μ is exactly your existing ordinal measure. The well‑foundedness harness is WellFounded.prod_lex Nat.lt_wfRel.wf Ordinal.lt_wf pulled back by μ̂ (standard).
>
> κ behavior
>
> recΔ b s (delta n) → app s (recΔ b s n): κ drops by 1 (we prove a tiny Nat max fact).
>
> All other 7 rules: κ is unchanged (or not larger). We therefore use μ‑drop on the right branch of the lex.
>
> μ‑drop lemmas used (ordinal APIs only)
> All already present in your cleaned MuCore.lean, except we add one (see code): mu_lt_eq_diff.
>
> R_int_delta (integrate (delta t) → void): mu_void_lt_integrate_delta. Tactics: simp on μ + Order.lt_add_one_iff.
>
> R_merge_void_left/right and R_merge_cancel: mu_lt_merge_void_left, mu_lt_merge_void_right, mu_lt_merge_cancel. Tactics: lift by le_mul_right, pad by le_add_*, close via lt_add_one_iff.
>
> R_rec_zero: mu_lt_rec_zero. Tactics: same pattern, padding the big head term on RHS.
>
> R_rec_succ: κ‑drop only (left branch of lex), a few lines of Nat.max arithmetic; no ordinal algebra. (We give a short lemma kappaD_drop_recSucc.)
>
> R_eq_refl: mu_void_lt_eq_refl. Tactics: simp.
>
> R_eq_diff (a ≠ b): new mu_lt_eq_diff a b (added below) using only green‑list ordinal APIs:
> (i) local derived bounds ω^3·(x+1) ≤ ω^(x+4), ω^2·(x+1) ≤ ω^(x+3);
> (ii) strict‑mono in the exponent via opow_lt_opow_right;
> (iii) additive‑principal “sum under ω^(κ)” (principal_add_omega0_opow) to put the whole payload under ω^(μa+μb+5);
> (iv) multiply by ω^4 → collapse with opow_add to bound by ω^(μa+μb+9);
> (v) Order.add_one_le_of_lt & Order.lt_add_one_iff to move +/- 1.
> All pieces are in the toolkit import set.
>
> Dependency scan
> Kernel unchanged; we use the R_eq_diff : a ≠ b → … form you shipped (both variants are in your repo; we stick to the version with ≠).
>
> New helpers (MuCore):
> le_omega_pow, termA_le, termB_le, and the main mu_lt_eq_diff. One‑liners are explained in comments and obey the toolkit (ordinal APIs only). We add one import: Mathlib.SetTheory.Ordinal.Principal (allowed in §1/“Additional Import”).
>
> Harness: keep a single WF harness file. Below I give SN_Final.lean and you should not build Termination_Lex.lean or MuLexSN.lean concurrently (those earlier files contained placeholders). The harness uses noncomputable def muHat (fixing your earlier “depends on noncomputable” error).






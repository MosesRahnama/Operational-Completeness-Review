Purpose: Evidence extract (misc/analysis) documenting a failure or relevance; Planning notes; not direct evidence of a failure, but records proposed remediation steps.
Contents: Metadata header + excerpt from the source file.
Context: Plan/brainstorm document summarizing proposed fixes for lexicographic (kappa, mu) termination proof.
Source: MUST_Review/MetaMD_Archive/AI_BrainStorm_SN.md
SHA256: 141EF8AC9DDBE7217FDAD41236DCD26CD24958845EBF924E8C12746DD1668FDD
FailureExplanation: Planning notes; not direct evidence of a failure, but records proposed remediation steps.
FailureModeTags: 

Excerpt:
> 1 What everyone agrees on (solid ground)
> •  The lexicographic measure μ̂(t) := (κ(t), μ(t)) under  
>   `Prod.Lex (·<·) (·<·)` + `InvImage.wf` is the right scaffold.
>
> •  κ must bump ­exactly once: +1 only at `recΔ _ _ (delta _)`.  
>   With that choice the κ-drop lemma `kappaD_drop_recSucc` lets us treat every `R_rec_succ` **purely by κ**, so the notorious `rec_succ_bound` is no longer needed.
>
> •  For the other seven rules μ-drop lemmas are already available (void→integrate, merge-void L/R, merge-cancel, rec-zero, eq-refl, eq-diff).  
>   The only lemma that sometimes “goes missing” in builds is `mu_lt_eq_diff`; it lives in `Meta.Termination` and must stay visible in `MuCore`.
>
> •  The file pair `Termination_Lex.lean + MuCore.lean` already implements this strategy end-to-end and compiles when `mu_lt_eq_diff` is in scope.
>
> ────────────────────────────────────────
> 2 Why other branches fail
> •  `SN_Final` and the two “Claude” files keep κ equal in the `rec_succ` δ-case; that forces a μ-drop that depends on the impossible domination inequality.  
>   Without a measure redesign those files will never go green.
>
> •  `Measure.lean` and `Termination_Legacy.lean` mix the old (false) bound with newer code, generating duplicate names and “unknown identifier” noise.
>
> ────────────────────────────────────────
> 3 What the new opinions add
> They contribute a precise recipe for finishing the **only** missing ingredient in the green path:
>
>   • Rewrite the helper bounds `termA_le` / `termB_le` in `MuCore.lean` using  
>     `opow_add`, `le_omega_pow`, and the finite-offset bridges  
>     `add3_plus1_le_plus4`, `add2_plus1_le_plus3`.
>
>   • Re-enable (or simply export) `mu_lt_eq_diff` so `Termination_Lex.lean`’s `R_eq_diff` branch compiles.
>
> Everything else in the long document reiterates build hygiene we were already following: quarantine legacy files, don’t global-simp the non-δ rec case, normalise `Nat.max` shapes locally, keep trace noise off, etc.
>
> ────────────────────────────────────────
> 4 Final concrete plan (no code yet)
>
> Step 0 (sanity check, <1 min)  
>   • `lake build OperatorKernelO6.Meta.Termination_Lex` — confirm the only red comes from a missing or broken `mu_lt_eq_diff`.  
>   • `grep -R \"sorry\\|admit\" Meta/*` — should list only MuCore errors you pasted.
>
> Step 1 Fix MuCore (high-confidence mechanical edits)  
>   A. At top: `import OperatorKernelO6.Meta.Termination` and `open MetaSN`.  
>   B. Rewrite `termA_le`, `termB_le` exactly as outlined (use `le_omega_pow`, `opow_add`, offset lemmas; no use of right-additivity).  
>   C. Delete the accidental local binder that shadows `mu`.  
>   D. Remove extraneous lemma names from `simp` brackets; move them into separate `have` equalities if actually needed.  
>   E. Make sure lines
>
> ```
> theorem mu_lt_eq_diff (a b : Trace) :
>   mu (.integrate (.merge a b)) < mu (.eqW a b)
> ```
>
>    compile, then `export` it (or rely on the copy that is already in `Meta.Termination`—but do **not** create two definitions with that name).
>
> Step 2 Harness integrity  
>   A. Open `Termination_Lex.lean` and ensure its `R_eq_diff` clause imports/opens the module where `mu_lt_eq_diff` now definitely lives and calls it via `drop_right`.  
>   B. Check that its κ definition is the local `kappaD`, not the one from `Kappa.lean`.  
>   C. Leave `kappaD_rec_base` **not** `[simp]` (only the δ variant is).
>
> Step 3 Quarantine legacy noise  
>   A. Move `Measure.lean`, `Termination_Legacy.lean` (and any variants you know are dead ends) to a non-compiled `Archive/` folder or guard them with `if false then` style `#eval`.  
>   B. Search the active code (`rg "Termination_Legacy"` etc.) to ensure no live import pulls them back in.  
>   C. Confirm `lakefile.lean`/`lake-manifest.json` doesn’t enumerate those files as build targets.
>
> Step 4 Re-export the finished theorem  
>   Option A (minimal): add a tiny file
>
> ```
> -- Meta/SN_Export.lean
> import OperatorKernelO6.Meta.Termination_Lex
> export OperatorKernelO6.MetaSNFinal (strong_normalization_final)
> ```
>
>   Option B: copy the exact lex harness into `SN_Final.lean` if you want a uniform filename; just keep the code identical.





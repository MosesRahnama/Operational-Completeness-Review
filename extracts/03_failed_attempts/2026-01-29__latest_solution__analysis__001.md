Purpose: Evidence extract (failed_attempts/analysis) documenting a failure or relevance; Duplicate of primary entry.
Contents: Metadata header + excerpt from the source file.
Context: Excerpt from latest_solution outlining termination-proof constraint blockers and a failed-attempts audit.
Source: Legacy/MetaMD_Archive/latest_solution.md
SHA256: FE25EA023EE8F8499B3759F8055702C35FD7564A825D64A3A9083C8D1C9C560A
FailureExplanation: The memo documents false ordinal lemmas and measure-invariant conflicts that leave the termination proof stuck under current constraints.
FailureModeTags: constraint_violation; invalid_rewrite; nondecreasing_measure; proof_obligation_stuck

Excerpt:
> constraint blocker (must fix before I can emit compiling code)
> CBâ€‘1 â€” Îº invariant vs. the seven rules is impossible as stated.
> The â€œnonâ€‘negotiable invariantâ€ says Îº strictly decreases only on recÎ”-succ; otherwise unchanged. In your kernel, the rules merge_void_leftright and rec_zero can change the topâ€‘level shape from a nonâ€‘recÎ” to a recÎ” (or remove it), so any Îº that detects recÎ” to drop on rec_succ will change on at least one other rule. Concrete witness set t = recÎ” b s n. The rule merge_void_right  merge t void â†’ t has Îº(source)=Îº(merge t void) and Îº(target)=Îº(t). If Îº ignores topâ€‘level recÎ” (to keep equal here), it cannot detect drop on rec_succ. If Îº does notice topâ€‘level recÎ” to drop on rec_succ, then here Îº(source)â‰ Îº(target). This is exactly what produced those â€œunsolved goals âŠ¢ Falseâ€ in your lex proof when it tried to force kappa (â€¦) = kappa (â€¦) for mergeâ€‘rules with a recÎ” argument.
> 
> Ask please confirm we can relax the invariant to â€œÎº strictly decreases on recÎ”-succ; on the other 7 rules it is nonâ€‘increasing, and we use Î¼â€‘drop when the Îºs are equal.â€
> This is the design your working â€œkappaDâ€ already follows (maxâ€‘depth of recs), and itâ€™s what the inâ€‘progress harness was trying to do with kappaD_drop_recSucc + â€œdrop_rightâ€ on equal Îº.
> 
> CBâ€‘2 â€” toolkit lemma opow_mul_lt_of_exp_lt (positivity version) is false.
> The version â€œÎ²  Î± â†’ 0  Î³ â†’ Ï‰^Î²  Î³  Ï‰^Î±â€ is not true (Î²=0, Î±=1, Î³=Ï‰ is a counterexample LHS = Ï‰ = RHS). The Mu proof attempt for Î¼_lt_eq_diff tries to invoke precisely this shape (screenshot + red squiggle under that call), which canâ€™t be rescued by typing tweaks. The correct, safe statement is the bounded form with Î³  Ï‰, or (better) avoid the product trick entirely and dominate the whole inner sum by a bigger Ï‰â€‘tower, which is exactly what your â€œtermA_le  termB_le â†’ omega_pow_add3_ltâ€ pipeline does.
> 
> Ask please accept the toolkit correction
> 
> keep only the bounded form (Î³  Ï‰) or switch Î¼_lt_eq_diff to the sumâ€‘domination route (no opow_mul_lt_of_exp_lt needed).
> The second route is already fully written in your meta patch (see â€œtermA_letermB_leomega_pow_add3_ltmerge_inner_bound_simpleâ€), and itâ€™s the one Iâ€™ll wire in.
> 
> CBâ€‘3 â€” Kernel rule eq_diff.
> Some of your files show Step.R_eq_diff  eqW a b â†’ integrate (merge a b) with a premise a â‰  b (e.g., old kernels), others state it unconditionally (newer spec). The acceptance list says â€œ8 unconditional rules (incl. eqâ€‘diff)â€. I need the unconditional version to avoid spurious case splits. Please confirm which version is authoritative. (If it is conditional, the Î¼â€‘drop lemma must be stated with that hypothesis, but SN proof is otherwise identical.)
> 
> CBâ€‘4 â€” Terminator harness shape mismatches (easy fixes once CBâ€‘1 is cleared).
> Your failing Termination_Lex uses WellFounded.prod_lex with a bad argument (fun x = lt_wf), and drop_leftright were declared but not definedâ€”these cause the â€œdeclaration is not a definitionâ€ and dependent elimination errors you saw. These are mechanical; Iâ€™ll fix them in the final patch.
> 
> what went wrong (brief triage with pointers)
> The Î¼â€‘proof for eqWâ€‘diff called a nonâ€‘existentincorrect lemma opow_mul_lt_of_exp_lt â€¦ (0  Î³); Lean rightfully complains about a type mismatch around the 0  Î¼(merge a b) + 1 hypothesis in your MuCore (screenshot). The correct path is the sum bound prove
> Î¼(merge a b) + 1  Ï‰^(Î¼ a + Î¼ b + 5) via termA_le, termB_le, then multiply by Ï‰^4 and push to Ï‰^(Î¼ a + Î¼ b + 9); this is exactly â€œmerge_inner_bound_simpleâ€ â†’ Î¼â€‘drop in eqWâ€‘diff.
> 
> The lex harness tried to force Îº equality for the three merge rules even when the argument is a recÎ”, creating impossible subgoals â€œâŠ¢ Falseâ€ (you can see the generated impossible cases in the error log). Thatâ€™s CBâ€‘1.
> 
> what Iâ€™m ready to ship once unblocked
> As soon as you confirm CBâ€‘1 (Îº nonâ€‘increasing except strict on recâ€‘succ) and CBâ€‘2 (toolkit correction), Iâ€™ll push a compiling set of patches with no sorry, no new axioms, and keeping universes tidy. The plan reuses the alreadyâ€‘drafted, working pieces from your meta bundle.
> 
> 1) failed attempts audit (oneâ€‘page table)
> Tag	Essential claim	Why it fails	Status
> A1	Use â€œpositivity onlyâ€ opow_mul_lt_of_exp_lt  Î²Î± â†’ 0Î³ â†’ Ï‰^Î²Î³  Ï‰^Î±	Mathematically false (Î²=0, Î±=1, Î³=Ï‰ is a counterexample) â†’ breaks Î¼â€‘drop for eqWâ€‘diff	DISALLOWED
> A2	Assume Î¼ s â‰¤ Î¼ (Î´ n) inside recÎ” b s n	False in general; counterexamples exist and are documented in your consolidation notes	DISALLOWED
> A3	Rely on strict monotonicity of right addition on ordinals	Not available; ordinal addition on the right is not strictly monotone	DISALLOWED
> A4	Prove SN with Î¼ only via rec_succ_bound	This lemma is openfragile; your own notes flag it as a nonâ€‘goal	DISALLOWED
> A5	Let Îº be a topâ€‘level â€œisâ€‘recÎ”â€ bit and require Îº equal on all nonâ€‘rec_succ rules	Fails on merge â€¦ â†’ â€¦ and rec_zero when arguments are (or become) recÎ” (see CBâ€‘1)	DISALLOWED
> 
> (Items A2A3 are explicitly called out in your consolidationtoolkit files; see the â€œcritical Î¼â€‘measure correctionâ€ section and related warnings.)





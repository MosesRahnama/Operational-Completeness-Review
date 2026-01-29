Context: Diagnostic report summarizing StrongNormal.lean errors and fixes.
Source: C:\Users\Moses\OpComp\MUST_Review\MetaMD_Archive\Diag_Reports.md
SHA256: C138CEF27FE2EF56AD7E2E7E849AF1A1A1A1797231FBFE4329BB5F7561658037
FailureExplanation: Diagnostic analysis; not direct evidence of model failure.
FailureModeTags: 

Excerpt:
> summary
> File mixes κ-lex harness patterns with local ordinal arithmetic; most errors are shape/equality normalizations and a few misapplied constructors.
> Two main clusters:
> κ-shape/defeq issues around kappaD for recΔ, and misuse of drop_left/drop_right.
> Ordinal arithmetic/equality shape mismatches in the eq_diff pipeline (Order.succ placement, ω-power product targets, reliance on unknown tactics).
> blockers by location
> Lines 154–156: kappaD defeq and rfl failure
> Errors
> “Not a definitional equality: kappaD (b.recΔ s n) …”
> “type mismatch rfl … expected kappaD (b.recΔ s n) = …”
> Diagnosis
> You’re using rfl where kappaD needs a case-split: the δ-case adds +1; the non-δ rec base is a max without +1. Without cases or a local lemma, the equation is not definitional.
> Fix direction
> Case on n (or use a lemma like kappaD_rec_base as in Termination_Lex: cases n; simp [kappaD]).
> Avoid rfl here; first normalize with Nat.max_assoc/left_comm/comm, then close.
> Lines 167–170: unsolved Nat.max equality and unused simp args
> Errors
> “unsolved goals: … max (κ b) (max (κ s) (κ n)) = T”
> Unused simp args: hs_le_T, Nat.max_eq_right hs_le_T
> “no goals to be solved” (extra simp after closed goal)
> Diagnosis
> Need a pure associativity/commutativity normalization on Nat.max to convert LHS to the definition of T.
> Fix direction
> Use Nat.max_assoc/left_comm/comm to rewrite LHS to T; keep Nat.max_eq_right hs_le_T only where it changes the term. Remove unused simp args; drop trailing simp after the goal is solved.
> Lines 186–192: drop_left/drop_right declarations and dependent elimination
> Errors
> “declaration is not a definition '… drop_left'/'… drop_right'”
> “dependent elimination failed … kappaD a = kappaD b”
> Diagnosis
> Likely redeclared lemma headers without bodies or with mismatched form; or using dependent pattern matching without providing the equality proof needed to rewrite.
> Fix direction
> Ensure drop_left/drop_right are actual lemmas with proofs (like in Termination_Lex: unfold Lex, apply Prod.Lex.left/right).
> For drop_right uses, compute hk : kappaD b = kappaD a via simp/normalization first, then pass hk (or hk.symm) explicitly.
> Lines 216–263: ordinal “+1” shape mismatches
> Errors
> “mu t < ω^3 + ω^2 * succ (mu t)” vs “ω^3 + succ (ω^2 * succ (mu t))”
> Similar mismatch later: “… + ω^2” vs “… + succ (ω^2)”
> Diagnosis
> Using Order.add_one_le_of_lt / Order.lt_add_one_iff inconsistently places succ; also mixing ≤ and < leads to add-left/right lemmas with different result shapes.
> Fix direction
> Choose one canonical “+1” bridge and stick to it for both sides (typically: make a strict bound A < B, then lift to A + 1 ≤ B and finish with lt_add_one_iff).
> Keep succ attached to the outermost sum consistently; avoid interleaving add_le_add_left with strict bounds unless you resolve succ placement immediately.
> Lines 321–337 and 332: eq_diff pipeline (targets and tactics)
> Errors
> “this has type ω^3 * succ A ≤ ω^3 * (ω^A * ω) but expected ω^A * ω^4”
> Unknown tactic (likely norm_num); type mismatch building 4 < B + 5; unsolved goals A+4 < C+5.
> Diagnosis
> Target RHS must be ω^(A+4), not ω^A * ω^4; rewrite products using Ordinal.opow_add to collapse ω^k * ω^(A+1) into a single ω-power.
> The 4 < B + 5 step must use Nat.cast + add_lt/add_le for ordinals—no norm_num.
> Fix direction
> For termA_le/termB_le: lift (x+1) ≤ ω^(x+1), multiply, rewrite via opow_add: ω^k * ω^(x+1) = ω^(k + (x+1)), then opow_le_opow_right to ω^(x+4)/ω^(x+3).
> For A+4 < C+5: prove (4:Ordinal) < B + 5 via Nat.cast_lt and le_add_left; then add_lt_add_left to get A + 4 < A + (B + 5) = C + 5 (normalize with add_*).
> Avoid norm_num; stick to ordinal-safe lemmas.
> Lines 498–503: unused simp args; lines 519: R_eq_diff arity
> Errors
> Unused simp arg: Nat.le_max_left
> “Invalid pattern: Too many arguments to 'R_eq_diff'; expected 1 explicit argument”
> Diagnosis
> The simp list includes names that do nothing; remove them.
> Step.R_eq_diff likely expects only the inequality witness as explicit argument; a and b are implicit or need named arguments exactly once.
> Fix direction
> Trim simp argument lists to only equalities/defs that rewrite.
> Use the constructor with correct arity (e.g., Step.R_eq_diff hneq, or Step.R_eq_diff (a:=a) (b:=b) hneq—but not both styles together).
> structural issues (κ and measure)
> kappaD recΔ equalities: You must case on n to justify either the +1 (delta case) or base max (non-delta) form. This mirrors Termination_Lex’s kappaD_rec_succ and kappaD_rec_base pattern.
> drop_left/drop_right: Should mirror the canonical versions in Termination_Lex (unfold LexNatOrd/muHat → Prod.Lex.left/right). Redeclarations with missing bodies or wrong namespace will error as “not a definition”.
> ordinal arithmetic consistency
> Targets:
> Use ω^(x+4)/ω^(x+3) for termA_le/termB_le; don’t aim for ω^x * ω^4.
> Collapse products with opow_add; then compare exponents via opow_le_opow_right (ω normality).
> “+1” bridges:
> Prefer: have h : X < Y → add-one bridge via Order.add_one_le_of_lt or Order.lt_add_one_iff consistently on one side; avoid mixing both directions mid-proof.
> A+4 < C+5:
> Build (4:Ordinal) < B + 5 from Nat facts lifted to ordinals, then add A on the left, then normalize C := A + B with add_assoc/comm.
> likely root causes
> Attempting rfl where only a case-split lemma applies (kappaD recΔ).
> Using simp with unused or irrelevant facts; failing to normalize Nat.max shapes first.
> Misapplying Step constructors (R_eq_diff) with wrong explicit args.
> Using unavailable tactics (norm_num) or shape-mismatched ordinal targets instead of the opow_add pathway.
> priority fixes (in order)
> κ equalities
> Add/use local lemmas for kappaD_rec_base and kappaD_rec_succ (or case on n) to avoid rfl failures.
> drop_left/drop_right
> Ensure they have concrete proofs (like in Termination_Lex) and are not redeclared conflictingly.
> Nat.max normalization goals
> Solve equalities with Nat.max_assoc/left_comm/comm and Nat.max_eq_right/left, outside simp.
> Ordinal “+1” shape mismatches
> Standardize to consistent succ placement using Order.add_one_le_of_lt / Order.lt_add_one_iff on one side only.
> eq_diff arithmetic
> Fix termA_le/termB_le with opow_add targets ω^(x+4)/ω^(x+3).
> Replace norm_num with Nat.cast-based ordinal steps.
> R_eq_diff arity
> Use the constructor with the right explicit/implicit parameter usage.
> non-blocking lint
> Unused variable hκ at line 94: rename to _hκ or use it.
> Unused simp args: remove from simp lists.

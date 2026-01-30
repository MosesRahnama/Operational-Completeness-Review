Purpose: Evidence extract (failed_attempts/analysis) documenting a failure or relevance; Duplicate of FRV2-0568.
Contents: Metadata header + excerpt from the source file.
Context: File review extract.
Source: MUST_Review/llm-flaws.txt
SHA256: 696667E8BAE678799E1DB61710D4B96B72CEFF2E00F4C82F75A1EB556FBF1DAE
FailureExplanation: Analysis or notes documenting failures, blockers, or unproven claims.
FailureModeTags: constraint_violation; nonconvergence

Excerpt:
> =====O3 "COMPREHENSIVE" ASSESSMENT=====
> Comprehensive Post-Mortem of the Strong-Normalization (SN) Project ==================================================================
> This is a chronological, cause-and-effect map of **every lane we tried**, where it broke, **why** it broke, and the mathematical lesson learned. Think of it as a diagnostic atlas to prevent us from re-entering any dead ends. -------------------------------------------------------------------- A. The Kernel & the One Troublemaker Rule -------------------------------------------------------------------- * Kernel is immutable: seven benign rules + the **R_rec_succ** rule
> rec b s ( n)    app s (rec b s n)
> The left argument (a rec) turns into a merge. Any measure has to **drop** across that transition; otherwise well-foundedness fails. -------------------------------------------------------------------- B. -Only Era (Ordinal Measure)  _rec_succ_bound _ -------------------------------------------------------------------- 1. **Idea**Define a non-computable ordinal  that drops on every rule. 2. **Sticking-Point**Needed an inequality
> (rec b s ( n))  >  (app s (rec b s n))
> which is *false* in general. We manufactured a lemma rec_succ_bound : (merge ) < (rec   _ ) and hid it behind a sorry. 3. **Outcome**First green build (Termination_Lex v0) was *illusory*. The sorry sat at the core; the moment we tried to remove it, the ordinal arithmetic collapsed. *Lesson*Ordinal  alone cannot distinguish has one more  on the RHS without extra structural information. -------------------------------------------------------------------- C.  = Structural Max-Depth (-Blind)  _+1 bump _ -------------------------------------------------------------------- 1. **Definition**
> (rec  ( n)) = base + 1
> (rec   n   ) = base       -- for all other n
> with base = max ( b) ( s) ( n). Everywhere else  just mirrors subterm maxima. 2. **Good News** decreases on seven of the eight rules. 3. **Failure Mode**If n itself is a  m, we have






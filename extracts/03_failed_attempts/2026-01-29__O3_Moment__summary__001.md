Purpose: Evidence extract (failed_attempts/analysis) documenting a failure or relevance; Analysis or notes documenting failures, blockers, or unproven claims.
Contents: Metadata header + excerpt from the source file.
Context: File review extract.
Source: MUST_Review/O3-Moment.md
SHA256: F0577D4BEA2D30B4296065A907AD174DAC8237079AA4DF343547287F2688CDE7
FailureExplanation: Analysis or notes documenting failures, blockers, or unproven claims.
FailureModeTags: constraint_violation; nonconvergence

Excerpt:
> =====O3 "COMPREHENSIVE" ASSESSMENT===== Comprehensive Post-Mortem of the Strong-Normalization (SN) Project ================================================================== This is a chronological, cause-and-effect map of **every lane we tried**, where it broke, **why** it broke, and the mathematical lesson learned. Think of it as a diagnostic atlas to prevent us from re-entering any dead ends. -------------------------------------------------------------------- A. The Kernel & the One Troublemaker Rule -------------------------------------------------------------------- * Kernel is immutable: seven benign rules + the **R_rec_succ** rule
> rec b s ( n)    merge s (rec b s n)
> The left argument (a rec) turns into a merge. Any measure has to **drop** across that transition; otherwise well-foundedness fails. -------------------------------------------------------------------- B. -Only Era (Ordinal Measure)  _rec_succ_bound _ -------------------------------------------------------------------- 1. **Idea**Define a non-computable ordinal  that drops on every rule. 2. **Sticking-Point**Needed an inequality
> (rec b s ( n))  >  (merge s (rec b s n))
> which is *false* in general. We manufactured a lemma rec_succ_bound : (merge ) < (rec   _ ) and hid it behind a sorry. 3. **Outcome**First green build (Termination_Lex v0) was *illusory*. The sorry sat at the core; the moment we tried to remove it, the ordinal arithmetic collapsed. *Lesson*Ordinal  alone cannot distinguish has one more  on the RHS without extra structural information. -------------------------------------------------------------------- C.  = Structural Max-Depth (-Blind)  _+1 bump _ -------------------------------------------------------------------- 1. **Definition**
> (rec  ( n)) = base + 1
> (rec   n   ) = base       -- for all other n
> with base = max ( b) ( s) ( n). Everywhere else  just mirrors subterm maxima. 2. **Good News** decreases on seven of the eight rules. 3. **Failure Mode**If n itself is a  m, we have
> (merge s (rec  n)) = base + 1 = (rec  ( n))
> so no strict drop  the lexicographic engine stalls. *Lesson*A constant +1 bump cannot see *nested* s. -------------------------------------------------------------------- D.  + 2, +3,   _Constant Escalation _ -------------------------------------------------------------------- 1. **Motivation**Try to create slack: base+2, base+3, etc. 2. **Result**Fails for exactly the same reason: when n =  m, the base itself is unaffected, so both sides rise by **the same constant**. Equality persists; no decrease. Any finite constant shift can be defeated by a second nested . *Lesson*Uniform additive constants cannot guarantee strict drop; we need **shape-sensitive** information. -------------------------------------------------------------------- E.  +  Lexicographic (Termination_Lex revamp) -------------------------------------------------------------------- 1. **Attempt**Keep  at +1, let  finish the job when  ties. 2. **Hurdle**Required resurrecting the *false* rec_succ_boundthe very thing we swore off. 3. **Status**Abandoned. *Lesson*Handing the problem back to  just re-introduces the falsity. -------------------------------------------------------------------- F.  + 2 with max- lemmas (KappaPlus2.lean) -------------------------------------------------------------------- 1. **Try**Proved helper kappa_drop_recSucc : (merge ) < (rec   _). 2. **Root Cause of Failure** The key inequality used





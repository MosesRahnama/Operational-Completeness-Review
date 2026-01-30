Purpose: Evidence extract (failed_attempts/analysis) documenting a failure or relevance; The document labels MuCore fully green while also listing missing rec_succ and deferred eq_diff proofs, showing termination support is incomplete.
Contents: Metadata header + excerpt from the source file.
Context: Excerpt from math_docs summarizing MuCore status and explicitly listing missing rec_succ and deferred eq_diff proofs.
Source: Legacy/MetaMD_Archive/math_docs.md
SHA256: 699B5EC35C5701875F3AA2B1607A63EAF302CFB5C829983EF52D02DFCDDB48BE
FailureExplanation: The document labels MuCore fully green while also listing missing rec_succ and deferred eq_diff proofs, showing termination support is incomplete.
FailureModeTags: inconsistency; unsupported_inference

Excerpt:
> ## FILE: MuCore.lean
> 
> **AI_REVIEW_SECTION**: MuCore.lean
> 
> ### Mathematical Content
> 
> **Purpose**: Core ordinal-valued measure μ : Trace → Ordinal and fundamental μ-decrease lemmas for 7 out of 8 kernel reduction rules.
> 
> **Status**: ✅ **ALL GREEN** - compiles without errors, admits, or sorry
> 
> ---
> 
> #### 1. The Ordinal Measure μ
> 
> The measure μ assigns to each `Trace` term a **Cantor Normal Form (CNF)** ordinal, designed to strictly decrease under 7 of the 8 kernel reduction rules:
> 
> ```lean
> μ : Trace → Ordinal
> μ(void)        = 0
> μ(delta t)     = ω^5 · (μ(t) + 1) + 1
> μ(integrate t) = ω^4 · (μ(t) + 1) + 1  
> μ(merge a b)   = ω^3 · (μ(a) + 1) + ω^2 · (μ(b) + 1) + 1
> μ(recΔ b s n)  = ω^(μ(n) + μ(s) + 6) + ω · (μ(b) + 1) + 1
> μ(eqW a b)     = ω^(μ(a) + μ(b) + 9) + 1
> ```
> 
> **Design principles**:
> - **Monotonicity**: Each constructor adds a "dominant term" ω^k times a larger expression
> - **Separation**: Different constructors use different ω-exponents (5, 4, 3, 2, dynamic) to ensure non-interference
> - **CNF structure**: Terms are ordered ω^α₁ + ω^α₂ + ... with α₁ > α₂ > ... for well-foundedness
> 
> ---
> 
> #### 2. Proven μ-Decrease Lemmas (7/8 rules)
> 
> The following strict inequalities are **mathematically proven** in MuCore.lean:
> 
> | **Rule** | **Rewrite** | **μ-inequality** | **Status** |
> |----------|-------------|------------------|------------|
> | R_int_delta | `integrate (delta t) → void` | `μ(void) < μ(integrate (delta t))` | ✅ |
> | R_merge_void_left | `merge void t → t` | `μ(t) < μ(merge void t)` | ✅ |
> | R_merge_void_right | `merge t void → t` | `μ(t) < μ(merge t void)` | ✅ |
> | R_merge_cancel | `merge t t → t` | `μ(t) < μ(merge t t)` | ✅ |
> | R_rec_zero | `recΔ b s void → b` | `μ(b) < μ(recΔ b s void)` | ✅ |
> | R_eq_refl | `eqW a a → void` | `μ(void) < μ(eqW a a)` | ✅ |
> | **R_rec_succ** | `recΔ b s (delta n) → app s (recΔ b s n)` | `μ(app s (recΔ b s n)) < μ(recΔ b s (delta n))` | ❌ **MISSING** |
> | R_eq_diff | `eqW a b → integrate (merge a b)` (a≠b) | Complex; handled by rank bits in lex measures | ⚠️ **DEFERRED** |
> 
> ---
> 
> #### 3. Mathematical Techniques Used
> 
> **Ordinal arithmetic foundations**:
> - `ω^k · (α + 1)` dominates any ordinal `β ≤ α` when `k ≥ 1`
> - Left-multiplication by `ω^k` preserves ordinal ordering: `α < β ⟹ ω^k·α < ω^k·β`
> - Additive padding: `α ≤ γ + α` for any `γ ≥ 0`
> 
> **Proof pattern** (exemplified in `mu_lt_merge_void_left`):
> 1. **Base step**: `μ(t) < μ(t) + 1` (ordinal successor)
> 2. **Amplification**: `μ(t) + 1 ≤ ω^k · (μ(t) + 1)` (multiplication dominance)
> 3. **Padding**: Add the "head term" of the target constructor
> 4. **Simplification**: Unfold μ definitions and apply transitivity
> 
> ---
> 
> #### 4. Current Limitations
> 
> **Missing proof**: The 8th rule `R_rec_succ` requires proving:
> ```
> μ(app s (recΔ b s n)) < μ(recΔ b s (delta n))
> ```
> 
> **Mathematical challenge**: The target's dominant term `ω^(μ(δn) + μ(s) + 6)` must dominate the source's structure `ω^3·(...) + ω^2·(...) + 1`. While `μ(δn) = ω^5·(μ(n)+1)+1` is much larger than any polynomial in `μ(n)`, the precise domination inequality requires complex ordinal tower analysis not yet completed.
> 
> **Consequence**: MuCore.lean provides the foundation for **lexicographic termination proofs** where a primary counter κ handles `R_rec_succ` and μ handles the remaining 7 rules.
> 
> ---
> 
> #### 5. Role in Overall Termination Strategy
> 
> MuCore.lean serves as the **ordinal component** in multi-component termination measures:
> 
> - **Pure μ approach**: Requires completing the missing `R_rec_succ` proof
> - **Lexicographic (κ,μ) approach**: Uses MuCore's 7 proven lemmas + a structural counter κ for the 8th rule
> - **Compatibility**: All MuCore lemmas remain valid regardless of the chosen primary counter design
> 
> **Import status**: MuCore.lean has minimal dependencies and compiles cleanly, making it a stable foundation for any termination proof strategy.
> 
> ---






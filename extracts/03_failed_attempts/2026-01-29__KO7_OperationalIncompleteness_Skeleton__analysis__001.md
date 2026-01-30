Purpose: Evidence extract (failed_attempts/analysis) documenting a failure or relevance; Duplicate of MUST_Review\Legacy\Meta_Lean_Archive\KO7_OperationalIncompleteness_Skeleton.lean.
Contents: Metadata header + excerpt from the source file.
Context: Excerpt from KO7_OperationalIncompleteness_Skeleton.lean showing the duplication stress test for rule r4 and the size non-decrease calculation.
Source: MUST_Review/Legacy/Meta_Lean_Archive/KO7_OperationalIncompleteness_Skeleton.lean
SHA256: D279C39C2A9D779DF8A029E46B8D51D28BD915891DC783B3281150973892815E
FailureExplanation: The additive size measure provably does not strictly decrease under the duplication rule r4 (mul (s x) y → add y (mul x y)), indicating a core termination obstacle.
FailureModeTags: nondecreasing_measure

Excerpt:
> /-! B) Duplication stress test for Rule r4.
>     Show additive non-drop first using `size`.
> -/
> def R4_before (x y : Term) : Term := mul (s x) y
> def R4_after  (x y : Term) : Term := add y (mul x y)
>
> /-- Raw size profile to exhibit non-decrease; computation by unfolding `size`. -/
> def R4_size_profile (x y : Term) : Nat × Nat := (size (R4_before x y), size (R4_after x y))
>
> /-
> Additive calculation (by hand; see Part A for the algebra):
>   size (mul (s x) y) = 2 + size x + size y
>   size (add y (mul x y)) = 2 + size x + 2 * size y
> Hence `size(after) = size(before) + size y`. No strict drop whenever `size y ≥ 1`.
> Only after switching to a robust base order (e.g., DM multiset/RPO with explicit precedence)
> can we prove each RHS piece is strictly < the removed LHS redex.
> -/





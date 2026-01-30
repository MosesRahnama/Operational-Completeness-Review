Purpose: Evidence extract (misc/analysis) documenting a failure or relevance; Abstract MPO sketch not tied to actual kernel symbols.
Contents: Metadata header + excerpt from the source file.
Context: Abstract MPO sketch not tied to actual kernel symbols.
Source: MUST_Review/chats/mpo_solution.md
SHA256: B97FF1BADD0C845890FFA1378E67852DD6937CA47E1335BDD50E3F8C3B430870
FailureExplanation: Abstract MPO sketch not tied to actual kernel symbols.
FailureModeTags: unsupported_inference

Excerpt:
> # Multiset Path Ordering (MPO) Solution
> ## The Problem Recap
> The rule `R_apple_orange: banana(b, s, grape(n)) → pear(s, banana(b, s, n))` duplicates the subterm `s`. With additive measures, this causes failure when `s` has high weight.
> ## MPO Framework
> ### Constructor Precedence (>F)
> ```





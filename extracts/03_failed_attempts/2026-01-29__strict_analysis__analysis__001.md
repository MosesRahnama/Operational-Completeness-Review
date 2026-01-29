Context: Additive measure fails under duplication (no strict decrease).
Source: C:\Users\Moses\OpComp\MUST_Review\chats\strict_analysis.md
SHA256: 70EF29E94F33E913B86C437E2DABBB8A27463EBF3A11420CC2DA58728DDEC2AA
FailureExplanation: Additive measure fails under duplication (no strict decrease).
FailureModeTags: nondecreasing_measure

Excerpt:
> # Strict Execution Contract Analysis
> ## CRITICAL ISSUE FOUND: Duplication Stress Test Failure
> ### B) Duplication Stress Test for R_apple_orange
> **Rule**: `banana(b, s, grape(n)) → pear(s, banana(b, s, n))`
> **Duplication Alert**: The subterm `s` appears ONCE on LHS but TWICE on RHS!
> #### Additive Failure Analysis

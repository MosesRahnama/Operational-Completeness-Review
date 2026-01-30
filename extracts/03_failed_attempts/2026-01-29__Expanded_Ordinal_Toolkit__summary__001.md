Purpose: Evidence extract (failed_attempts/analysis) documenting a failure or relevance; Analysis or notes documenting failures, blockers, or unproven claims.
Contents: Metadata header + excerpt from the source file.
Context: File review extract.
Source: MUST_Review/Expanded_Ordinal_Toolkit.md
SHA256: C7F256CD4459BB896F8161FE6EB4903B2D62FFD4AE7A8CCCD134D31BD9F47289
FailureExplanation: Analysis or notes documenting failures, blockers, or unproven claims.
FailureModeTags: constraint_violation

Excerpt:
> # Expanded Ordinal Toolkit - Complete Reference
> This document consolidates all information on ordinal tactics, syntax, lemma construction, patterns, best practices, and failed attempts. All content is presented verbatim from source documents.
> ---
> ## 1. IMPORT & LIBRARY AUDIT (Authoritative)
> > Use exactly these modules; the righthand column clarifies *what is found where*. Generic orderedmonoid lemmas must **not** be used for ordinal multiplication unless explicitly noted.
> | Area                          | Correct import                                   | Contains / Notes                                                                                                                                                 |
> | ----------------------------- | ------------------------------------------------ | ---------------------------------------------------------------------------------------------------------------------------------------------------------------- |
> | WF/Acc                        | `Init.WF`                                        | `WellFounded`, `Acc`, `InvImage.wf`, `Subrelation.wf`                                                                                                            |
> | Prod lex orders               | `Mathlib.Data.Prod.Lex`                          | `Prod.Lex` for lexicographic measures                                                                                                                            |
> | Ordinal basics                | `Mathlib.SetTheory.Ordinal.Basic`                | `omega0_pos`, `one_lt_omega0`, `lt_omega0`, `nat_lt_omega0`                                                                                                      |





Context: Author notes/tables; not a proof artifact.
Source: C:\Users\Moses\OpComp\MUST_Review\important_2\Rahnama_KO7_tables_revised.md
SHA256: E8E2E171A5DA4ECC05E3010428EBE63967F592F0F72B9D0A9751B3A950561F44
FailureExplanation: Author notes/tables; not a proof artifact.
FailureModeTags: 

Excerpt:
> # Revised Tables Appendix (KO7 safe)
> ## Table R1 — KO7 Core Rules (code-accurate)
> | ID | LHS | RHS | Orientation component used | Guards |
> |---|---|---|---|---|
> | R1 | merge void t | t | μ-right | δ(t)=0 (propagates) |
> | R2 | merge t void | t | μ-right | δ(t)=0 (propagates) |
> | R3 | merge t t | t | μ-right after tie | **SafeStep**: δ(t)=0 ∧ κ(t)=0 |
> | R4 | recΔ b s void | b | κ-left (DM) | δ(b)=0 |
> | R5 | recΔ b s (delta n) | app s (recΔ b s n) | **δ-left (1→0)** | none |
> | R6 | integrate (delta t) | void | μ-right (κ rfl) | — |
> | R7 | eqW a a | void | μ-right after tie | **SafeStep**: κ(a)=0 |
> | R8 | eqW a b (a≠b) | integrate (merge a b) | μ-right | — |

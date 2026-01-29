Context: File review extract.
Source: C:\Users\Moses\OpComp\MUST_Review\target_theorem.md
SHA256: AFA8498CD2EDBB990D9FDC2AD18014CEAC6D2AC637F4677A0291A4660710765E
FailureExplanation: Reference or planning document; not a proof artifact.
FailureModeTags: 

Excerpt:
> # Target Theorem (initial skeleton)
> _This file will hold the evolving proof we are working on.  For now it is just a placeholder so that future edits can patch it with minimal diffs._
> ---
> # Ordinal Termination Proof: `tail_lt_A`
> ## Theorem statement
> ```lean
> lemma tail_lt_A {b s n : Trace} :
> let A : Ordinal := omega0 ^ (mu (delta n) + mu s + 6)
> omega0 ^ (2 : Ordinal) * (mu (rec b s n) + 1) < A
> ```

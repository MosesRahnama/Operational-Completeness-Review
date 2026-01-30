Purpose: Evidence extract (failed_attempts/analysis) documenting a failure or relevance; Analysis or notes documenting failures, blockers, or unproven claims.
Contents: Metadata header + excerpt from the source file.
Context: File review extract.
Source: MUST_Review/5th-6th candidates.md
SHA256: 816760FA51AFED2D43AC70E7B940FFBEB6523B942179258AD06B6CEAE6E3175A
FailureExplanation: Analysis or notes documenting failures, blockers, or unproven claims.
FailureModeTags: constraint_violation

Excerpt:
> 5th-6th candidates
> 5th candidates
> Label	Informal Role	Power Gain	Complexity	Risks to Minimality	Solves
> R (Primitive Recursor)	R base step n (unary chain iterator)	High (PR closure)	Moderate	Adds arithmetic bias	C14, C23
> EqNat co
> Iter (Fold / Catamorphism)	fold t f acc
> general structural fold	Very High (arbitrary structural recursion)	High	May look like cheating (meta recursion smuggled in)	Wide: su enumera
> Mu (WellFounded Fixpoint)	mu F seed reduces while measure drops	High (internal diagonal & derivability)	High	Harder termination proof	Diagonal
> EqW (Equality Witness)	eqW a b  void iff nf(a)=nf(b)` else canonical nonvoid	Medium	Low	Focused, simple	Removes DecEq re
> Search (Bounded Existential)	seek B P tries P(0..B)	Medium	LowMed	Narrow semantics	 prov e provabili





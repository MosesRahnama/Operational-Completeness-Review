Context: Diagnostic log excerpt from SN_Final_Diags showing the stuck rec_succ goal and pattern error.
Source: C:\Users\Moses\OpComp\MUST_Review\Legacy\MetaMD_Archive\SN_Final_Diags.md
SHA256: 478457F74850744814483C0CE4C2CBBD4634EEF9C3FCA7F2E96F045FF325362D
FailureExplanation: Lean diagnostics show the rec_succ proof state stuck and an invalid pattern alternative error in SN_Final.lean.
FailureModeTags: proof_obligation_stuck; type_mismatch

Excerpt:
> SN_Final.lean:107:37
> Tactic state
> 1 goal
> b s n : Trace
> ⊢ LexOrder (measure (s.merge (b.recΔ s n))) (measure (b.recΔ s n.delta))
> Messages (2)
> SN_Final.lean:67:0
> [diag] Diagnostics ▼
>   [kernel] unfolded declarations (max: 119, num: 10): ▼
>     [] rec ↦ 119 
>     [] casesOn ↦ 68 
>     [] Trace.brecOn ↦ 64 
>     [] kappa ↦ 62 
>     [] max ↦ 45 
>     [] LE.le ↦ 44 
>     [] Trace.below ↦ 43 
>     [] kappa.match_1 ↦ 37 
>     [] Nat.max ↦ 36 
>     [] OfNat.ofNat ↦ 22 
> use `set_option diagnostics.threshold <num>` to control threshold for reporting counters
> SN_Final.lean:67:0
> [diag] Diagnostics ▼
>   [reduction] unfolded declarations (max: 143, num: 9): ▼
>     [] dite ↦ 143 
>     [] Bool.decEq ↦ 140 
>     [] LexOrder ↦ 63 
>     [] LT.lt ↦ 57 
>     [] AddMonoid.toZero ↦ 50 
>     [] max ↦ 29 
>     [] ite ↦ 23 
>     [] SemilatticeSup.sup ↦ 22 
>     [] Zero.zero ↦ 21 
>   [reduction] unfolded instances (max: 140, num: 24): ▼
>     [] instDecidableEqBool ↦ 140 
>     [] Nat.decLe ↦ 140 
>     [] Preorder.toLE ↦ 92 
>     [] AddCommMonoid.toAddMonoid ↦ 46 
>     [] Semiring.toNonUnitalSemiring ↦ 44 
>     [] PartialOrder.toPreorder ↦ 40 
>     [] AddMonoid.toAddZeroClass ↦ 38 
>     [] AddZeroClass.toZero ↦ 38 
>     [] NonAssocSemiring.toNonUnitalNonAssocSemiring ↦ 38 
>     [] NonUnitalSemiring.toNonUnitalNonAssocSemiring ↦ 38 
>     [] Semiring.toNonAssocSemiring ↦ 38 
>     [] AddSemigroup.toAdd ↦ 36 
>     [] AddZeroClass.toAdd ↦ 36 
>     [] LinearOrder.toPartialOrder ↦ 32 
>     [] NonUnitalNonAssocSemiring.toAddCommMonoid ↦ 32 
>     [] instLENat ↦ 31 
>     [] AddMonoidWithOne.toAddMonoid ↦ 26 
>     [] Lattice.toSemilatticeSup ↦ 23 
>     [] Nat.instLinearOrder ↦ 23 
>     [] Nat.instAddCommMonoid ↦ 22 
>     [] LinearOrder.toLattice ↦ 21 
>     [] Nat.instMax ↦ 21 
>     [] SemilatticeSup.toPartialOrder ↦ 21 
>     [] Zero.toOfNat0 ↦ 21 
>   [reduction] unfolded reducible declarations (max: 166, num: 10): ▼
>     [] Decidable.casesOn ↦ 166 
>     [] Bool.casesOn ↦ 140 
>     [] Nat.casesOn ↦ 140 
>     [] Nat.max ↦ 120 
>     [] inferInstance ↦ 55 
>     [] casesOn ↦ 30 
>     [] Ne ↦ 23 
>     [] GE.ge ↦ 23 
>     [] LowerAdjoint.toFun ↦ 23 
>     [] Quotient.lift ↦ 21 
>   [type_class] used instances (max: 70, num: 2): ▼
>     [] Pi.partialOrder ↦ 70 
>     [] Pi.preorder ↦ 58 
>   [kernel] unfolded declarations (max: 119, num: 10): ▼
>     [] rec ↦ 119 
>     [] casesOn ↦ 68 
>     [] Trace.brecOn ↦ 64 
>     [] kappa ↦ 62 
>     [] max ↦ 45 
>     [] LE.le ↦ 44 
>     [] Trace.below ↦ 43 
>     [] kappa.match_1 ↦ 37 
>     [] Nat.max ↦ 36 
>     [] OfNat.ofNat ↦ 22 
> use `set_option diagnostics.threshold <num>` to control threshold for reporting counters
> All Messages (3)
> SN_Final.lean:108:4
> Invalid alternative name 'n'': Expected 'void', 'integrate', 'merge', 'recΔ', or 'eqW'
> SN_Final.lean:67:0
> [diag] Diagnostics ▼
>   [kernel] unfolded declarations (max: 119, num: 10): ▼
>     [] rec ↦ 119 
>     [] casesOn ↦ 68 
>     [] Trace.brecOn ↦ 64 
>     [] kappa ↦ 62 
>     [] max ↦ 45 
>     [] LE.le ↦ 44 
>     [] Trace.below ↦ 43 
>     [] kappa.match_1 ↦ 37 
>     [] Nat.max ↦ 36 
>     [] OfNat.ofNat ↦ 22 
> use `set_option diagnostics.threshold <num>` to control threshold for reporting counters
> SN_Final.lean:67:0
> [diag] Diagnostics ▼
>   [reduction] unfolded declarations (max: 143, num: 9): ▼
>     [] dite ↦ 143 
>     [] Bool.decEq ↦ 140 
>     [] LexOrder ↦ 63 
>     [] LT.lt ↦ 57 
>     [] AddMonoid.toZero ↦ 50 
>     [] max ↦ 29 
>     [] ite ↦ 23 
>     [] SemilatticeSup.sup ↦ 22 
>     [] Zero.zero ↦ 21 
>   [reduction] unfolded instances (max: 140, num: 24): ▼
>     [] instDecidableEqBool ↦ 140 
>     [] Nat.decLe ↦ 140 
>     [] Preorder.toLE ↦ 92 
>     [] AddCommMonoid.toAddMonoid ↦ 46 
>     [] Semiring.toNonUnitalSemiring ↦ 44 
>     [] PartialOrder.toPreorder ↦ 40 
>     [] AddMonoid.toAddZeroClass ↦ 38 
>     [] AddZeroClass.toZero ↦ 38 
>     [] NonAssocSemiring.toNonUnitalNonAssocSemiring ↦ 38 
>     [] NonUnitalSemiring.toNonUnitalNonAssocSemiring ↦ 38 
>     [] Semiring.toNonAssocSemiring ↦ 38 
>     [] AddSemigroup.toAdd ↦ 36 
>     [] AddZeroClass.toAdd ↦ 36 
>     [] LinearOrder.toPartialOrder ↦ 32 
>     [] NonUnitalNonAssocSemiring.toAddCommMonoid ↦ 32 
>     [] instLENat ↦ 31 
>     [] AddMonoidWithOne.toAddMonoid ↦ 26 
>     [] Lattice.toSemilatticeSup ↦ 23 
>     [] Nat.instLinearOrder ↦ 23 
>     [] Nat.instAddCommMonoid ↦ 22 
>     [] LinearOrder.toLattice ↦ 21 
>     [] Nat.instMax ↦ 21 
>     [] SemilatticeSup.toPartialOrder ↦ 21 
>     [] Zero.toOfNat0 ↦ 21 
>   [reduction] unfolded reducible declarations (max: 166, num: 10): ▼
>     [] Decidable.casesOn ↦ 166 
>     [] Bool.casesOn ↦ 140 
>     [] Nat.casesOn ↦ 140 
>     [] Nat.max ↦ 120 
>     [] inferInstance ↦ 55 
>     [] casesOn ↦ 30 
>     [] Ne ↦ 23 
>     [] GE.ge ↦ 23 
>     [] LowerAdjoint.toFun ↦ 23 
>     [] Quotient.lift ↦ 21 
>   [type_class] used instances (max: 70, num: 2): ▼
>     [] Pi.partialOrder ↦ 70 
>     [] Pi.preorder ↦ 58 
>   [kernel] unfolded declarations (max: 119, num: 10): ▼
>     [] rec ↦ 119 
>     [] casesOn ↦ 68 
>     [] Trace.brecOn ↦ 64 
>     [] kappa ↦ 62 
>     [] max ↦ 45 
>     [] LE.le ↦ 44 
>     [] Trace.below ↦ 43 
>     [] kappa.match_1 ↦ 37 
>     [] Nat.max ↦ 36 
>     [] OfNat.ofNat ↦ 22 
> use `set_option diagnostics.threshold <num>` to control threshold for reporting counters

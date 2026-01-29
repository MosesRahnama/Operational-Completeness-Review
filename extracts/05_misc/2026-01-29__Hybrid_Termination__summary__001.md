Context: File review extract.
Source: C:\Users\Moses\OpComp\MUST_Review\Hybrid_Termination.md
SHA256: 5698103894EB694B70446BAFD3D855246472F2D0604D3F45227AFD4B8194465F
FailureExplanation: Reference or planning document; not a proof artifact.
FailureModeTags: 

Excerpt:
> # Hybrid Termination Artifacts (KO7 + MPO)
> This note summarizes the publishable termination artifacts consolidated in `OperatorKernelO6/Meta/Termination_KO7.lean`.
> Scope and claims:
> - Per-step hybrid decrease: every `Kernel.Step a b` strictly decreases either the MPO-style measure `3m` (-first) or the KO7 triple-lex measure `3`.
> - Formal: `MetaSN_Hybrid.hybrid_drop_of_step : Step a b  HybridDec a b`.
> - Safe subrelations are well-founded in reverse:
> - KO7-safe: `MetaSN_KO7.wf_SafeStepRev : WellFounded MetaSN_KO7.SafeStepRev`.
> - MPO-safe: `MetaSN_MPO.wf_SafeStepMPORev : WellFounded MetaSN_MPO.SafeStepMPORev`.
> - We do not assert a single global measure over the entire `Step` due to duplication and right-add hazards (ordinal addition). Instead, we expose the hybrid certificate and two safe well-founded subsets. This is intentional and documented.
> > Canonical copy moved to `Submission_Material/md/Hybrid_Termination.md`.

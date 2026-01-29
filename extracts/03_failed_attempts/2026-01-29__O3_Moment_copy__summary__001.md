Context: File review extract.
Source: C:\Users\Moses\OpComp\MUST_Review\O3-Moment copy.md
SHA256: 85126A2ACE357442EE659A10B6874F194BC9371D0337D0F07BDA0C786EDF62C3
FailureExplanation: Analysis or notes documenting failures, blockers, or unproven claims.
FailureModeTags: constraint_violation; nonconvergence

Excerpt:
> # Cursor O3-Gold
> # Comprehensive Post-Mortem of the Strong-Normalization (SN) Project
> This is a chronological, cause-and-effect map of **every lane we tried**, where it broke, **why** it broke, and the mathematical lesson learned.  Think of it as a diagnostic atlas to prevent us from re-entering any dead ends.
> ---
> ## A.  The Kernel & the One Troublemaker Rule
> * Kernel is immutable: seven benign rules + the **R_rec_succ** rule
> ```
> rec b s ( n)    merge s (rec b s n)
> ```
> The left argument (a `rec`) turns into a `merge`.  Any measure has to **drop** across that transition; otherwise well-foundedness fails.

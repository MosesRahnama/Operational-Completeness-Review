Context: Post-mortem emphasizes measure drop across rec_succ rule.
Source: C:\Users\Moses\OpComp\MUST_Review\chats\O3-Moment.md
SHA256: C30CF52E516D887D03E4CA95ED9F53A4AF0E26112333CFA552C5073D6C127452
FailureExplanation: Post-mortem emphasizes measure drop across rec_succ rule.
FailureModeTags: nondecreasing_measure

Excerpt:
> # Comprehensive Post-Mortem of the Strong-Normalization (SN) Project
> This is a chronological, cause-and-effect map of **every lane we tried**, where it broke, **why** it broke, and the mathematical lesson learned.  Think of it as a diagnostic atlas to prevent us from re-entering any dead ends.
> ---
> ## A.  The Kernel & the One Troublemaker Rule
> * Kernel is immutable: seven benign rules + the **R_rec_succ** rule
> ```

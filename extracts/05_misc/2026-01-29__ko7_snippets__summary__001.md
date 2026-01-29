Context: Snippet collection; not a proof artifact.
Source: C:\Users\Moses\OpComp\MUST_Review\important_2\ko7_snippets.txt
SHA256: 22944CC09973602B0492A1163F53C7A9EC77386C10837AEAF57CE0E34068BCFA
FailureExplanation: Snippet collection; not a proof artifact.
FailureModeTags: 

Excerpt:
> ==== 01_Kernel.md ====
> 13: | integrate : Trace → Trace
> 14: | merge : Trace → Trace → Trace
> 16: | recΔ : Trace → Trace → Trace → Trace
> 17: | eqW : Trace → Trace → Trace
> 22: | R_int_delta : ∀ t, Step (integrate (delta t)) void
> 23: | R_merge_void_left : ∀ t, Step (merge void t) t
> 24: | R_merge_void_right : ∀ t, Step (merge t void) t
> 25: | R_merge_cancel : ∀ t, Step (merge t t) t
> 26: | R_rec_zero : ∀ b s, Step (recΔ b s void) b
> 27: | R_rec_succ : ∀ b s n, Step (recΔ b s (delta n)) (app s (recΔ b s n))
> 28: | R_eq_refl : ∀ a, Step (eqW a a) void

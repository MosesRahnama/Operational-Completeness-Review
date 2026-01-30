Purpose: Evidence extract (misc/guide) documenting a failure or relevance; Guidance or index document; not a proof artifact.
Contents: Metadata header + excerpt from the source file.
Context: File review extract.
Source: MUST_Review/context_playbook.md
SHA256: 47131B394AAD1AC0D8CE6AC5B817037223117A56A4FAE4019645572F26134A66
FailureExplanation: Guidance or index document; not a proof artifact.
FailureModeTags: 

Excerpt:
> # Keeping the prompt under the context-length ceiling
> ## 1  Put the growing proof in a file
>  Maintain the evolving proof in this repository instead of the chat (e.g. in `proofs/target_theorem.md`).
>  Use `edit_existing_file` so only diffs are echoed, keeping prompts short.
>  The full proof never counts against the token budgetonly incremental changes do.
> ## 2  Summarize chat history regularly
>  After a few exchanges, condense the discussion (questions, clarifications, decisions) into a short bullet list.
>  Old verbose turns can then be pruned by the frontend or simply not re-pasted.
> ## 3  Avoid repeating tool schemas & instructions
>  The tool list and always include language + file rule live in system/developer messages; do **not** quote them again inside prompts.





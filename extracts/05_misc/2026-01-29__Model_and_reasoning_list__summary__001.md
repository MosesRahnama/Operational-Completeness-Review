Purpose: Evidence extract (misc/reference) documenting a failure or relevance; Background research memo; not a proof artifact.
Contents: Metadata header + excerpt from the source file.
Context: Background research memo; not a proof artifact.
Source: MUST_Review/important_2/Research/Model_and_reasoning_list.md
SHA256: F28C4E8250925D8F9B114B7D1A926B1057EEA040EC8FA57F91C47ECE2B7A7807
FailureExplanation: Background research memo; not a proof artifact.
FailureModeTags: 

Excerpt:
> # Model and reasoning list
> *Converted from: Model and reasoning list.docx*
> ---
> ## Latest Generative Models and Reasoning Options (Sep 1 2025)
> This report summarises the newest large‑language‑model offerings from OpenAI, Anthropic (Claude) and Google’s Gemini platform with a focus on their reasoning features. Citations are included so that you can check the sources yourself. Models are listed from most capable to more budget‑friendly options.
> ### OpenAI: GPT‑5 and the O‑series
> OpenAI’s GPT‑5 and O‑series models run on the Chat Completions or Responses API. They are designed to automatically switch between standard and reasoning mode; developers can still control the reasoning effort via parameters.
> | Model | Reasoning/Thinking options | Key features (context window etc.) | Release notes |
> | --- | --- | --- | --- |
> | GPT‑5 (latest flagship) | reasoning_effort can be minimal, low, medium or high[1]; minimal is a new option for quick answers; preamble allows a plan to be produced before tool calls[1] | ~272 k input and 128 k output tokens for a 400 k context window[2]. Built‑in thinking reduces hallucinations (45 % less errors than GPT‑4o and 80 % less than O3 when thinking is enabled)[3]. Supports multi‑modal inputs (images/videos), function calls, code execution and structured outputs[2]. | Announced July 2025; default mode automatically chooses between standard and reasoning. Personality presets (cynic, robot, listener, nerd) and improved writing/coding[3]. |
> | gpt‑oss‑120b / gpt‑oss‑20b (open‑weight models) | Do not expose a reasoning‑effort parameter; reasoning must be implemented by developers manually. | 131 k token context window (much smaller than GPT‑5)[4]. Multi‑modal inputs not supported[4]. | Released Aug 2025 as Apache 2.0–licensed open weights; designed to run on a single GPU or laptop for local control[5]. Intended for developers who need to fine‑tune or self‑host. |
> | O‑series models | General: O‑models use simulated reasoning that allows the model to think internally before responding; you can set reasoning_effort to low, medium or high (O‑3 mini also supports minimal and high variants). Higher effort uses more time and tokens, generating deeper reasoning[6].[7]. | Models share 128 k to 272 k token context windows depending on size. All support function calls, images/videos and tool use. O3‑Pro adds web search and Python execution[8]. | O‑3: base reasoning model, generally available Apr 16 2025[9]. O‑3 mini: cost‑efficient version with low, medium and high reasoning variants; high variant consumes more tokens and time[6]. O‑3 pro: highest reasoning and reliability; slower but can search web, analyse files and run Python[8]; released Jun 10 2025[9]. O‑4 mini / O‑4 mini‑high: improved cost‑efficiency over O‑3 mini with two variants (standard and high reasoning); released Apr 16 2025[10]. |





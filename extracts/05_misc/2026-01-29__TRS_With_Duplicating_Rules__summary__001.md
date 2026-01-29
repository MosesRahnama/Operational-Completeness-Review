Context: Background research memo; not a proof artifact.
Source: C:\Users\Moses\OpComp\MUST_Review\important_2\Research\TRS_With_Duplicating_Rules.md
SHA256: 7020EED9EADCFB7759EE134E8BC925F9A7FD2BAA17EC2AC9C87B618A7FD18EBD
FailureExplanation: Background research memo; not a proof artifact.
FailureModeTags: 

Excerpt:
> # TRS With Duplicating Rules
> *Converted from: TRS With Duplicating Rules.docx*
> ---
> Selected TRSs (TPDB) – We choose ten TRSs with duplicating rules (variables occur more often on RHS) or base-change rules (constructor/head-symbol changes), including classic hard cases from SK90 and D33[1][2] and two relative TRSs. The table below summarizes the results. All runs used TTT2 (120s timeout) and AProVE, with CeTA validating each CPF proof. (For two cases, we also independently certified the proof via CiME3 by generating a Coq script and compiling it[3] – indicated in notes.) All CPF proofs were accepted by CeTA. (In early competitions, some proofs contained unsupported steps (e.g. replacing 0-arity symbols with unary for string-reversal[4]), yielding an unknownProof that CeTA rejects[5]. No such issues occurred here.) The KO7-like duplicating pattern dup(t) → c(t,t) (two copies on RHS) exemplifies rules that require sophisticated measures (e.g. multiset orders or DP framework) for orientation[6].
> | TRS (TPDB) | Dup/Base? | Techniques (Tool Output) | Tool Verdict | CPF | CeTA | Notes |
> | --- | --- | --- | --- | --- | --- | --- |
> | SK90/4.27 (Steinbach 1990)[1] | base-change (int→list) | LPO with status (precedence int > cons oriented all rules) | YES (TTT2 & AProVE) | SK90-4.27.cpf | Accepted | Classic nested integer-to-list recursion; simple lexicographic path order suffices.[7] CiME3 check ✔️ |
> | SK90/2.46 | duplicating | Poly–interpretation + DP (matrix weights) | YES (TTT2 & AProVE) | SK90-2.46.cpf | Accepted | Required numeric ranking to handle variable duplication (TTT2 used DP > matrix). |
> | D33/13 (Dershowitz 1995)[2] | duplicating | Matrix interp. (dim 3) & DP graph | YES (proved) | D33-13.cpf | Accepted | Hard example (#13 of 33[2]); solved via dependency pairs + linear matrix order (multiset extension). |
> | D33/33 (Dershowitz) | base-change + dup | DP with multiset order + arctic poly | YES (proved) | D33-33.cpf | Accepted | Final “open problem” example[2]; needed DP framework with a Dershowitz–Manna multiset component (handle two recursive calls). |
> | Endrullis_06/pair2simple2 | duplicating | KBO (weight tweaks) + DP fallback | YES (proved) | pair2simple2.cpf | Accepted | Involves a pair(x,y) function duplicating arguments; oriented by weight order (KBO)[6] after DP preprocessing. |
> | AG01/3.19 (Arts-Giesl 2000)[8] | duplicating | Dependency Pairs + lex order | YES (TTT2 & AProVE) | AG01-3.19.cpf | Accepted | First solved by the DP method[8] (LPO alone fails); AProVE’s proof uses DP with a reduction pair. |

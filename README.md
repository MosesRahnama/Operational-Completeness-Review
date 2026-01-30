# Operational Completeness and the Boundary of Intelligence

**Single‑source evidence base for the Operational Completeness paper**

Purpose: Orient readers and provide the definitive map to the evidence in this repository.
Contents: Repository overview, key finding, navigation instructions, and citation.
Contents: Repository overview, key finding, navigation instructions, and citation.

This repository is the structured extraction of ~5 months of proof attempts, strict tests, diagnostics, and model transcripts related to the Operational Completeness project. Every reviewed item is indexed with metadata and a one‑sentence failure explanation, so readers can verify the paper’s claims directly from the evidence.
The original raw artifacts are not included in this repo because they are noisy and too large. Full source material can be made available to referees and reviewers upon request (e.g., via a shared drive link).

## How this repo relates to the paper
The paper argues that “operational completeness” fails for the targeted operator‑only system because termination breaks at the same structural point across models and strategies. This repo documents those failures with file‑level provenance, excerpts, and failure‑mode tags, so the paper’s statements can be traced and audited.

## Start here (fast path)
1) `maps/category_map.md` — definitions of the categories.
2) `maps/failure_mode_legend.md` — the failure‑mode taxonomy used throughout.
3) `logs/review_log.csv` — the master log (every file, with metadata and paths).
4) `maps/file_map.csv` — browse by path, category, relevance.
5) `logs/repo_manifest.csv` — manifest of every repo file with purpose and metadata.

## Repository structure (navigation)
- `logs/review_log.csv` — canonical per‑file log (status, metadata, extract/full include paths).
- `maps/file_map.csv` — file map for browsing and categorization.
- `extracts/` — per‑file excerpts with context and failure explanations.
  - `01_transcripts/` — transcripts (Category 1)
  - `02_tests/` — strict test runs and summaries (Category 2)
  - `03_failed_attempts/` — failed proof attempts and diagnostics (Category 3)
  - `05_misc/` — misc/planning/reference (Categories 4–5)
- `full_includes/` — full copies for selected high‑value documents.
- `templates/` — templates used to format extracts and full includes.

## Key finding: the rec_succ rule
The system’s reduction rule
```lean
R_rec_succ : ∀ b s n, Step (recΔ b s (δ n)) (merge s (recΔ b s n))
```
combines:
1) **Self‑reference** — `recΔ` appears on both sides
2) **Duplication** — `s` appears once on the LHS, twice on the RHS
3) **Decidability pressure** — termination must strictly drop

For any additive measure M:
```
M(after) = M(before) − 1 + M(s)
When M(s) ≥ 1 → NO STRICT DROP
```
This pattern blocks every termination strategy attempted in the evidence.

## Evidence summary (from the master log)
| Category | Files |
| --- | --- |
| Transcripts | 11 |
| Tests | 6 |
| Failed attempts | 347 |
| Misc / reference | 339 |

Counts are derived from `logs/review_log.csv`.

## How to interpret the logs
- `FailureExplanation` — one‑sentence explanation of the failure mechanism or relevance.
- `FailureMode` — tags from `maps/failure_mode_legend.md`.
- `ExtractPath` — pointer to the excerpt file (if present).
- `FullIncludePath` — pointer to a full copy if included.
- `DuplicateOf` — links duplicates to the canonical record (duplicates still point to canonical extract/full include paths).

## What counts as evidence
Evidence includes failed Lean proof attempts, diagnostic error traces, strict test runs, model transcripts, and internal reports showing concrete failure modes. The goal is not a single narrative but a comprehensive, verifiable map of how and where attempts fail.

## Instant falsification
This project is **instantly falsifiable**: if any AI system successfully proves termination for the full `Step` relation without hidden axioms (no `sorry` in Lean), the central claim is refuted.

## Citation
```bibtex
@article{rahnama2026opcomp,
  title={Operational Completeness and the Boundary of Intelligence: An Empirical Theory of Universal Cognitive Limitations in Large Language Models},
  author={Rahnama, Moses},
  year={2026},
  note={Preprint}
}
```

## License
Apache 2.0 — see `LICENSE`.

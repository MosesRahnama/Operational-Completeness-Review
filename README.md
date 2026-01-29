# Operational Completeness Review

This repository contains the full, file-by-file review of `C:\Users\Moses\OpComp\MUST_Review` (fresh review: 2026‑01‑29).

Start here:
- `maps/category_map.md` — what each category means.
- `maps/failure_mode_legend.md` — canonical failure-mode tags.
- `logs/review_log.csv` — canonical per-file log.

## Structure
- `maps/file_map.csv` — file map for quick browsing and categorization.
- `logs/review_log.csv` — canonical per-file log (status, metadata, extract/full include paths).
- `extracts/` — per-file excerpts with context and failure explanations.
  - `01_transcripts/` — chat transcripts (Category 1)
  - `02_tests/` — test runs and summaries (Category 2)
  - `03_failed_attempts/` — failure artifacts (Category 3)
  - `05_misc/` — misc/planning/reference (Categories 4–5)
- `full_includes/` — full copies for selected high-value documents (instead of excerpts).

## How to read the logs
- `FailureExplanation` gives the 1‑sentence meta explanation for every reviewed item.
- `FailureMode` uses the legend in `maps/failure_mode_legend.md`.
- `ExtractPath` points to the excerpt file (if present).
- `FullIncludePath` points to a full copy when included instead of excerpt.
- `DuplicateOf` (in `review_log.csv`) points to the canonical file when a file is a duplicate; duplicate rows still point to the canonical extract/full include for easy navigation.

## Notes
- Some documents are stored as full copies (see `full_includes/`) and therefore may not have an excerpt.
- Some PDFs are non‑text/encoded and require OCR; these are flagged in their notes.

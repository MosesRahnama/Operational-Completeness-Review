# Reviewer Guide

Purpose: Provide a fast verification path and a deep‑audit path for reviewers.
Contents: Step‑by‑step audit flow, key checks, and how to trace evidence in this repo.

## Quick path (15–30 minutes)
1) Read `README.md` for orientation and the core claim.
2) Open `maps/failure_mode_legend.md` and `maps/category_map.md`.
3) Open `logs/review_log.csv` and spot‑check a few high‑relevance rows.
4) Open the linked files in `ExtractPath` / `FullIncludePath` and verify the failure explanations.

## Full path (deep audit)
1) Read `README.md`.
2) Review `maps/category_map.md` and `maps/failure_mode_legend.md`.
3) Use `maps/file_map.csv` to browse by category and relevance.
4) For each claim you care about, trace it to the specific evidence in `extracts/` or `full_includes/`.
5) Use `logs/review_log.csv` to map back to the original source path and hash.

## What to verify
- The rec_succ duplication issue is supported by primary excerpts.
- Failures repeat across multiple models and protocols.
- Strict test runs are recorded and consistent with the conclusions.
- Each evidence item includes a one‑sentence `FailureExplanation`.

## Access to original artifacts
The original raw artifacts are not stored in this repo because they are noisy and too large. Full source material can be shared with referees and reviewers upon request (e.g., via a shared drive link).

## Evidence naming scheme
Each evidence file name follows:

```
YYYY-MM-DD__slug__type__NNN.md
```

Use `logs/review_log.csv` to trace any extract or full include back to the original source path and hash.

# Evidence Map

Purpose: Explain how to locate evidence and interpret the repo structure after extraction.
Contents: Scope policy, navigation map, and the primary indices for tracing sources.

## Scope and source policy
This repo is a standalone evidence base. All items are indexed with source paths and hashes in `logs/review_log.csv` so reviewers can trace any excerpt back to the original artifact. The original raw artifacts are not stored here because they are noisy and too large, but they can be made available to referees and reviewers upon request (e.g., via a shared drive link).

## How to find a file
Use `maps/file_map.csv` to browse by path/category, or `logs/review_log.csv` to jump directly to `ExtractPath` or `FullIncludePath`.

## Key folders
- `extracts/` — short excerpts with metadata headers (primary evidence).
- `full_includes/` — full copies for high‑value documents.
- `maps/` — category and failure‑mode definitions + file map.
- `logs/` — master logs and repo manifest.

## Index files
- `logs/review_log.csv` — master per‑file log (source path, category, failure mode, extract/full include).
- `maps/file_map.csv` — browsing map keyed by repo path.
- `logs/repo_manifest.csv` — manifest of every repo file with purpose and metadata.

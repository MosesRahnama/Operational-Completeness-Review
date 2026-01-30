Purpose: Evidence extract (failed_attempts/analysis) documenting a failure or relevance; Analysis or notes documenting failures, blockers, or unproven claims.
Contents: Metadata header + excerpt from the source file.
Context: File review extract.
Source: MUST_Review/LAKE_BUILD_OPTIMIZATION.md
SHA256: 7EB30360BAEDF7E6C14F3AEF08B1C38185715DFE20A0B320915EB8BB735208EC
FailureExplanation: Analysis or notes documenting failures, blockers, or unproven claims.
FailureModeTags: constraint_violation; nonconvergence

Excerpt:
> # Lake Build Optimization Guide
> ##  **Problem**: Lake keeps rebuilding 3000+ mathlib files
> This happens because:
> 1. Cache invalidation when `.lake` folder is deleted
> 2. Broken import files trigger full rebuilds
> 3. No pre-built cache available for your mathlib version
> ##  **Solution**: Quick Build Script
> Save this as `quick-build.ps1`:
> ```powershell
> # Kill any running Lean processes





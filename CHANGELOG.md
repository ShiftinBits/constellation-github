# Changelog

All notable changes to the Constellation Index GitHub Action will be documented in this file.

The format is based on [Keep a Changelog](https://keepachangelog.com/en/1.1.0/),
and this project adheres to [Semantic Versioning](https://semver.org/spec/v2.0.0.html).

## [Unreleased]

### Fixed

- The diff check now compares against the last commit Constellation indexed for the branch (read from the project state API) instead of `github.event.before`. A never-indexed project whose setup commit touched no source files is now indexed, and source changes from a run that skipped or failed are picked up by the next run. If the project state cannot be read, the action indexes. ([SB-1247](https://linear.app/shiftinbits/issue/SB-1247/github-action-skips-indexing-for-a-never-indexed-project-when-the-push), [#6](https://github.com/ShiftinBits/constellation-github/pull/6))
- The diff check no longer misses a tracked source file renamed to an untracked extension (for example `a.ts` to `a.md`). Rename detection is now disabled, so the old path counts as a change and triggers indexing.

### Changed

- `schedule` runs now go through the same diff check and skip when nothing relevant changed since the last index. Set `skip-diff-check: "true"` to always index on schedule.

## [1.3.0] - 2026-05-28

### Added

- Self-healing git history: on `push`, the action detects a shallow clone and runs `git fetch --unshallow` so the diff check can resolve the push baseline and skip indexing when no tracked files changed. Best-effort and non-fatal — falls back to a full index if history cannot be fetched.

### Changed

- README now recommends `fetch-depth: 0` on `actions/checkout` and documents the `persist-credentials` interaction.

## [1.2.2] - 2026-04-22

### Changed

- Manual `workflow_dispatch` runs now bypass `diff_check` and force a full re-index.

## [1.2.1] - 2026-04-22

### Changed

- LSP servers are now installed globally (`npm install -g`) to fix enrichment failures spawning `typescript-language-server` and avoid stale runner binaries.
- Node.js setup and Constellation CLI install steps are skipped entirely when diff check determines indexing is not needed, reducing CI time on non-code commits.

### Fixed

- README links that pointed to incorrect repositories.

**Full Changelog**: https://github.com/ShiftinBits/constellation-github/compare/v1.2.0...v1.2.1

## [1.1.0] - 2026-04-09

### Added

- Smart diff detection: automatically skips indexing when no files matching `constellation.json` configuration have changed
- New `skip-diff-check` input to bypass diff detection and always run indexing
- Handles edge cases: first push, scheduled/manual triggers, shallow clones, missing config

## [1.0.0] - 2026-04-06

### Added

- Initial release of Constellation Index GitHub Action
- Privacy-first design: only structural metadata transmitted, never source code
- Single required input: `access-key`
- Outputs: `indexed` (boolean) and `summary` (string)
- Support for Ubuntu, macOS, and Windows runners

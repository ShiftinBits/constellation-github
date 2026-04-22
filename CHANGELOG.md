# Changelog

All notable changes to the Constellation Index GitHub Action will be documented in this file.

The format is based on [Keep a Changelog](https://keepachangelog.com/en/1.1.0/),
and this project adheres to [Semantic Versioning](https://semver.org/spec/v2.0.0.html).

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

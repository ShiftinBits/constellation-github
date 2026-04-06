# Changelog

All notable changes to the Constellation Index GitHub Action will be documented in this file.

The format is based on [Keep a Changelog](https://keepachangelog.com/en/1.1.0/),
and this project adheres to [Semantic Versioning](https://semver.org/spec/v2.0.0.html).

## [1.0.0] - 2026-04-06

### Added

- Initial release of Constellation Index GitHub Action
- Privacy-first design: only structural metadata transmitted, never source code
- Single required input: `access-key`
- Outputs: `indexed` (boolean) and `summary` (string)
- Support for Ubuntu, macOS, and Windows runners

# Changelog

All notable changes to the Constellation Index GitHub Action will be documented in this file.

The format is based on [Keep a Changelog](https://keepachangelog.com/en/1.1.0/),
and this project adheres to [Semantic Versioning](https://semver.org/spec/v2.0.0.html).

## [1.0.0] - 2024-12-22

### Added

- Initial release of Constellation Index GitHub Action
- Composite action that installs latest `@constellationdev/cli`
- Runs `constellation index` to extract metadata intelligence
- Single required input: `constellation-access-key`
- Outputs: `indexed` (boolean) and `summary` (string)
- Support for Ubuntu, macOS, and Windows runners
- Professional documentation with usage examples
- Security policy and guidelines

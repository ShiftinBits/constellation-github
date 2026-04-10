# Constellation Index Action

[![GitHub Action](https://img.shields.io/badge/GitHub-Action-blue?logo=github)](https://github.com/ShiftinBits/constellation-cli)
[![License: MIT](https://img.shields.io/badge/License-MIT-yellow.svg)](LICENSE)

## Overview

This GitHub Action installs the latest [Constellation CLI](https://github.com/ShiftinBits/constellation-cli) and indexes your repository. Constellation extracts structural intelligence from your codebase using Tree-sitter AST analysis, enabling AI assistants to understand your code without transmitting source code.

### Key Features

- **Privacy-First**: Only AST metadata is extracted - source code never leaves your repository
- **Automatic Updates**: Always uses the latest CLI version
- **Simple Integration**: Single input required - just your access key
- **Cross-Platform**: Runs on Ubuntu, macOS, and Windows runners
- **Smart Diff Detection**: Pair with the [diff check action](#smart-diff-detection) to skip indexing when no relevant files changed

## Quick Start

```yaml
- uses: ShiftinBits/constellation-github@v1
  with:
    access-key: ${{ secrets.CONSTELLATION_ACCESS_KEY }}
```

## Usage

### Basic Example

```yaml
name: Constellation Index

on:
  push:
    branches: [main]

permissions:
  contents: read

jobs:
  index:
    runs-on: ubuntu-latest
    steps:
      - uses: actions/checkout@v4

      - uses: ShiftinBits/constellation-github@v1
        with:
          access-key: ${{ secrets.CONSTELLATION_ACCESS_KEY }}
```

See a [live implementation example](https://github.com/ShiftinBits/constellation-cli/actions/workflows/constellation.yml) in the Constellation CLI repository.

### Smart Diff Detection

Use the companion `check` action to skip indexing when no relevant files have changed. When skipped, the index step shows a native **Skipped** status in the GitHub Actions UI.

```yaml
name: Constellation Index

on:
  push:
    branches: [main]

permissions:
  contents: read

jobs:
  index:
    runs-on: ubuntu-latest
    steps:
      - uses: actions/checkout@v4

      - uses: ShiftinBits/constellation-github/check@v1
        id: diff-check

      - uses: ShiftinBits/constellation-github@v1
        if: steps.diff-check.outputs.should_index == 'true'
        with:
          access-key: ${{ secrets.CONSTELLATION_ACCESS_KEY }}
```

The check action reads your `constellation.json` and compares the git diff against your configured `languages` and `exclude` patterns. If none of the changed files match a configured file extension (or all matches are excluded), `should_index` is set to `false` and the index step is skipped.

**The check action always signals `should_index=true` when:**
- This is the first push to a branch (no baseline to diff against)
- The trigger is `schedule` or `workflow_dispatch` (no push event context)
- The git diff fails (e.g., shallow clone) — falls back to indexing with a warning
- `skip-diff-check` is set to `"true"`

**A `constellation.json` file is required** in the repository root. The check action will fail if it is missing.

### Scheduled Indexing

```yaml
name: Constellation Index (Scheduled)

on:
  schedule:
    - cron: '0 2 * * *' # Daily at 2 AM UTC
  workflow_dispatch: # Manual trigger

permissions:
  contents: read

jobs:
  index:
    runs-on: ubuntu-latest
    steps:
      - uses: actions/checkout@v4

      - uses: ShiftinBits/constellation-github@v1
        with:
          access-key: ${{ secrets.CONSTELLATION_ACCESS_KEY }}
```

### Using Outputs

```yaml
- uses: ShiftinBits/constellation-github@v1
  id: constellation
  with:
    access-key: ${{ secrets.CONSTELLATION_ACCESS_KEY }}

- name: Check Result
  run: |
    echo "Indexed: ${{ steps.constellation.outputs.indexed }}"
    echo "Summary: ${{ steps.constellation.outputs.summary }}"
```

## Inputs

### Index Action (`ShiftinBits/constellation-github`)

| Input             | Required | Default  | Description                                                                         |
| ----------------- | -------- | -------- | ----------------------------------------------------------------------------------- |
| `access-key`      | Yes      | —        | Constellation API access key for authentication. Store this as a repository secret. |
| `error-reporting` | No       | `"true"` | Enable error reporting to Constellation in the event of indexing failures.           |

### Check Action (`ShiftinBits/constellation-github/check`)

| Input             | Required | Default   | Description                                                                    |
| ----------------- | -------- | --------- | ------------------------------------------------------------------------------ |
| `skip-diff-check` | No       | `"false"` | Skip the diff check and always signal indexing. Set to `"true"` to disable.    |

## Outputs

### Index Action

| Output    | Description                                                  |
| --------- | ------------------------------------------------------------ |
| `indexed` | `true` if indexing completed successfully, `false` otherwise |
| `summary` | Human-readable summary of the indexing operation             |

### Check Action

| Output         | Description                                              |
| -------------- | -------------------------------------------------------- |
| `should_index` | `true` if relevant files changed, `false` otherwise      |

## Permissions

This action requires:

| Permission | Level  | Reason                                   |
| ---------- | ------ | ---------------------------------------- |
| `contents` | `read` | Read repository files for AST extraction |

```yaml
permissions:
  contents: read
```

## Setup

### 1. Get an Access Key

Sign up at [constellationdev.io](https://constellationdev.io) to obtain your API access key.

### 2. Add Repository Secret

1. Go to your repository **Settings** > **Secrets and variables** > **Actions**
2. Click **New repository secret**
3. Name: `CONSTELLATION_ACCESS_KEY`
4. Value: Your Constellation API access key
5. Click **Add secret**

### 3. Add Workflow

Create `.github/workflows/constellation.yml` with one of the examples above.

## Requirements

- **Node.js**: 24.x (automatically set up by the index action)
- **Runner**: Ubuntu, macOS, or Windows
- **jq**: Pre-installed on all GitHub-hosted runners (used by the check action)

## Troubleshooting

### Authentication Errors

```
Error: Authentication failed
```

Verify your `CONSTELLATION_ACCESS_KEY` secret is correctly set and has not expired.

### Network Errors

```
Error: Connection refused
```

The Constellation API may be temporarily unavailable. The action will retry automatically. If the issue persists, check [status.constellationdev.io](https://status.constellationdev.io).

### Parse Errors

```
Error: Failed to parse file
```

Some files may contain syntax not yet supported by Constellation. These files are skipped, and indexing continues for supported files.

## Security

- **No Source Transmission**: Only AST metadata is transmitted - never source code
- **Encrypted Secrets**: Access keys are stored encrypted in GitHub Secrets
- **Minimal Permissions**: Only `contents: read` permission required

See [SECURITY.md](SECURITY.md) for our security policy and vulnerability reporting.

## Contributing

Contributions are welcome! Please see the [Constellation CLI repository](https://github.com/ShiftinBits/constellation-cli) for contribution guidelines.

## License

This project is licensed under the MIT License - see the [LICENSE](LICENSE) file for details.

## Support

- **Documentation**: [docs.constellationdev.io](https://docs.constellationdev.io)
- **Issues**: [GitHub Issues](https://github.com/ShiftinBits/constellation-github/issues)
- **Website**: [constellationdev.io](https://constellationdev.io)

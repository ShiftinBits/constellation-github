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

### Index on changes to main

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

| Input        | Required | Description                                                                         |
| ------------ | -------- | ----------------------------------------------------------------------------------- |
| `access-key` | Yes      | Constellation API access key for authentication. Store this as a repository secret. |

## Outputs

| Output    | Description                                                  |
| --------- | ------------------------------------------------------------ |
| `indexed` | `true` if indexing completed successfully, `false` otherwise |
| `summary` | Human-readable summary of the indexing operation             |

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

- **Node.js**: 24.x (automatically set up by the action)
- **Runner**: Ubuntu, macOS, or Windows

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

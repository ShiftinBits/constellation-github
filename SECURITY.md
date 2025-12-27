# Security Policy

## Supported Versions

| Version | Supported          |
| ------- | ------------------ |
| 1.x     | :white_check_mark: |

## Reporting a Vulnerability

We take security seriously. If you discover a security vulnerability in this GitHub Action, please report it responsibly.

### How to Report

1. **Do not** open a public GitHub issue for security vulnerabilities
2. Email hello@shiftinbits.com with:
   - Description of the vulnerability
   - Steps to reproduce
   - Potential impact

### What to Expect

- **Acknowledgment**: Within 48 hours
- **Initial Assessment**: Within 7 days
- **Resolution Timeline**: Depends on severity, typically within 30 days

### Scope

This security policy covers:

- The Constellation Index GitHub Action (`action.yml`)
- Associated scripts and configuration files in this directory

For vulnerabilities in the Constellation CLI itself, please report to the [main repository](https://github.com/ShiftinBits/constellation-cli).

## Best Practices

1. **Pin to a version tag** (e.g., `@v1`) rather than `@main`
2. **Review workflow permissions** before enabling
3. **Audit secrets access** in your repository settings
4. **Enable Dependabot** for security updates

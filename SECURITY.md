# Security Policy

The smbCloud team takes security vulnerabilities seriously. We appreciate your efforts to responsibly disclose your findings, and we will make every effort to acknowledge your contributions.

## Supported Versions

This action is versioned by Git tag. Security patches are provided for the latest major version tag (e.g. `v1`). Please pin to a major version tag (or a specific release) and keep it up to date.

| Version | Supported          |
| ------- | ------------------ |
| `v1`    | :white_check_mark: |
| `< v1`  | :x:                |

## Reporting a Vulnerability

If you discover a security vulnerability, please report it to us by emailing **smbcloud@setoelkahfi.se**.

**Please do not report security vulnerabilities through public GitHub issues.**

### What to Include in a Vulnerability Report

Please include the following details in your report:

- A clear and concise description of the vulnerability.
- The version (tag or commit SHA) of the action affected.
- Steps to reproduce the vulnerability. This could include a workflow snippet or a link to a repository with a proof of concept.
- The potential impact of the vulnerability.
- Any potential mitigations or workarounds you are aware of.

### Our Process

1.  When you report a vulnerability, we will acknowledge receipt of your report within 48 hours.
2.  We will investigate the report and determine if it is a valid security issue.
3.  If the vulnerability is accepted, we will work on a patch for the latest version as soon as possible.
4.  We will notify you when the patch is released.
5.  We will publicly disclose the vulnerability after the patch has been released, and we will credit you for the discovery unless you prefer to remain anonymous.

## Handling Secrets

This action reads sensitive inputs (`token`, `ssh-private-key`). Always pass these from [GitHub Actions secrets](https://docs.github.com/en/actions/security-guides/encrypted-secrets). Never hardcode them in a workflow file. The action writes them to the runner filesystem with `600` permissions and leaves cleanup to the ephemeral runner. Use a dedicated smbCloud token for CI with only the access it needs.

We value your contributions to keeping smbCloud secure.

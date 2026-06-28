# Contributing to smbcloud-deploy-action

Thank you for considering contributing! This is a [composite GitHub Action](https://docs.github.com/en/actions/creating-actions/creating-a-composite-action).
The whole thing is `action.yml` plus the bash it runs and the docs around it. There's no build step and no compiler.

## Code of Conduct

By participating in this project you agree to abide by our [Code of Conduct](CODE_OF_CONDUCT.md). Please be respectful and considerate of others.

## Prerequisites

- A POSIX shell with `bash` (the action's steps are `shell: bash`).
- [`actionlint`](https://github.com/rhysd/actionlint) lints workflow and action YAML.
- [`shellcheck`](https://www.shellcheck.net/) lints the bash inside the steps (actionlint runs it for you when installed).
- [`yq`](https://github.com/mikefarah/yq) or any YAML parser, for a quick syntax check.

## Getting Started

1. Fork the repository on GitHub.
2. Clone your fork:
   ```sh
   git clone https://github.com/your-username/smbcloud-deploy-action.git
   cd smbcloud-deploy-action
   ```
3. Create a branch for your work:
   ```sh
   git checkout -b your-feature-or-fix
   ```

## Development Workflow

### Validate `action.yml`

Confirm the YAML parses and lint the embedded shell:

```sh
# YAML parses
python3 -c "import yaml; yaml.safe_load(open('action.yml'))"

# Lint the action and its embedded bash (runs shellcheck under the hood)
actionlint
```

### Test it end to end

The only real test is running the action in a workflow. Add or update an example
workflow under `.github/workflows/` (or test from a throwaway repo) and trigger a
deploy against a disposable smbCloud project. Verify:

- the correct release binary is downloaded for the runner OS/arch,
- the token lands in the right state dir (`.smb` for production, `.smb-dev` for dev),
- a failed `smb deploy` fails the job (non-zero exit propagates).

## Coding Guidelines

These rules are enforced in code review:

- Keep every step `shell: bash` and start scripts with `set -euo pipefail`.
- Pass inputs into steps via `env:` and reference them as `"${INPUT_NAME}"`. Do
  **not** interpolate `${{ inputs.* }}` directly inside a `run:` block, since that
  invites shell injection. The action already follows this pattern; keep it.
- Quote all variable expansions.
- Fail fast with `echo "::error::..."` and a non-zero exit on bad input.
- Never log secret values. Token and key contents must not be echoed.
- Keep the action generic: no server hostnames, account/user IDs, real customer
  domains, project IDs, or other smbCloud fleet detail. Use placeholders
  (`example.com`, `<user-id>`, `<app>`) in docs and examples.

## Submitting a Pull Request

1. Ensure the YAML parses and `actionlint` is clean before pushing.
2. Update `README.md` if you add, rename, or change an input or output.
3. Push your branch and open a PR against `main`.
4. A maintainer will review your changes and may request modifications.

### PR Title Convention

Use a clear, imperative title that describes the change:

- ✅ `Add support for arm64 runners`
- ✅ `Fail fast when ssh-key-name is missing`
- ❌ `feat: add arm64` (no conventional-commit prefixes)
- ❌ `Fixed some stuff.` (no past tense, no trailing punctuation)

## Releasing

Releases are handled by maintainers via Git tags. Cut a version tag and move the
floating major tag so consumers pinning `@v1` get the update:

```sh
git tag -a v1.2.0 -m "v1.2.0"
git tag -f v1 v1.2.0
git push origin v1.2.0
git push -f origin v1
```

## Reporting Bugs

Use the GitHub issue tracker and fill out the bug report template. Please include:

- A clear description of the problem.
- A minimal workflow snippet that reproduces it.
- The runner OS, the action version (tag/SHA), and the resolved CLI version.
- Expected vs. actual behaviour.

## Requesting Features

Use the GitHub issue tracker and fill out the feature request template. Describe
the use case clearly: what problem does the feature solve?

## Security

Please report security vulnerabilities privately. See [SECURITY.md](SECURITY.md).
Do not open a public issue for security reports.

## Questions?

Open a GitHub issue or reach out to the maintainers. We're happy to help.

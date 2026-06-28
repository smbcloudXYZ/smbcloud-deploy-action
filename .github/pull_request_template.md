## Related Issue

Fixes # <!-- issue number, or remove this line if not applicable -->

## Description

<!-- What does this PR do? Why is this change needed? -->

## Type of Change

<!-- Mark the relevant option with an [x] -->

- [ ] Bug fix
- [ ] New feature (new input/output or behavior)
- [ ] Refactor (no functional change)
- [ ] Documentation update
- [ ] CI / tooling change

## How Has This Been Tested?

<!-- Describe how you exercised the action -->

- [ ] `action.yml` parses (YAML)
- [ ] `actionlint` is clean
- [ ] Ran the action in a workflow against a disposable smbCloud project

## Checklist

- [ ] Every step is `shell: bash` and starts with `set -euo pipefail`
- [ ] Inputs are passed via `env:` and referenced as `"${INPUT_*}"` (no direct `${{ inputs.* }}` in `run:`)
- [ ] All variable expansions are quoted
- [ ] No secret values are logged
- [ ] No smbCloud fleet detail (hosts, account/user IDs, real domains, project IDs) — placeholders only
- [ ] `README.md` updated if inputs/outputs changed

## Release Notes

<!-- One bullet. Use "- Added ...", "- Fixed ...", or "- Improved ..." for user-facing changes. Use "- N/A" otherwise. -->

-

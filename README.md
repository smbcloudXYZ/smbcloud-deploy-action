# smbCloud Deploy Action

[![GitHub Marketplace](https://img.shields.io/badge/Marketplace-smbCloud%20Deploy-blue?logo=github)](https://github.com/marketplace/actions/smbcloud-deploy)
[![Lint](https://github.com/smbcloudXYZ/smbcloud-deploy-action/actions/workflows/lint.yml/badge.svg)](https://github.com/smbcloudXYZ/smbcloud-deploy-action/actions/workflows/lint.yml)
[![License: Apache-2.0](https://img.shields.io/badge/license-Apache%202.0-blue.svg)](LICENSE)

A composite GitHub Action that installs the [smbCloud CLI](https://github.com/smbcloudXYZ/smbcloud-cli)
(`smb`) and deploys your app non-interactively from CI.

It wraps the CLI's `--ci` (non-interactive) deploy flow: provision a token,
optionally install an SSH key, then run `smb --ci deploy`. Your `.smb/config.toml`
picks the deploy strategy (`nextjs-ssr`, `vite-spa`, `rails`, `rust`, `swift`, or
generic rsync/git), so the same step works for any stack.

## Quick start

```yaml
name: Deploy
on:
  push:
    branches: [main]

jobs:
  deploy:
    runs-on: ubuntu-latest
    steps:
      - uses: actions/checkout@v7
      - uses: smbcloudXYZ/smbcloud-deploy-action@v1
        with:
          token: ${{ secrets.SMB_TOKEN }}
```

That installs the latest CLI, writes the token, and runs `smb --ci deploy`
against the project pinned in your `.smb/config.toml`.

## Monorepo

For a monorepo config with several `[[projects]]` entries, name the target.
Non-interactive mode won't prompt you to choose:

```yaml
      - uses: smbcloudXYZ/smbcloud-deploy-action@v1
        with:
          token: ${{ secrets.SMB_TOKEN }}
          project: my-web-app
```

## SSH / rsync deploys

Deploy paths that upload over rsync need the account-scoped SSH key. smbCloud
expects it at `~/.ssh/id_<user-id>@smbcloud`:

```yaml
      - uses: smbcloudXYZ/smbcloud-deploy-action@v1
        with:
          token: ${{ secrets.SMB_TOKEN }}
          ssh-private-key: ${{ secrets.SMB_SSH_KEY }}
          ssh-key-name: id_<user-id>@smbcloud
          ssh-known-hosts: ${{ secrets.SMB_KNOWN_HOSTS }}
```

Pin `ssh-known-hosts` instead of disabling host-key checking.

## Inputs

| Input               | Required | Default                  | Description                                                                                          |
| ------------------- | -------- | ------------------------ | ---------------------------------------------------------------------------------------------------- |
| `token`             | yes      | _(required)_             | smbCloud auth token contents (from a CI secret). Written to the CLI state dir to authenticate `smb`. |
| `environment`       | no       | `production`             | `production` or `dev`. Selects API host and state dir (`.smb` vs `.smb-dev`).                         |
| `project`           | no       | `""`                     | Monorepo sub-project name (the `name` in a `[[projects]]` entry).                                     |
| `working-directory` | no       | `.`                      | Directory containing `.smb/config.toml`.                                                              |
| `version`           | no       | `latest`                 | CLI version to install (e.g. `0.5.0`, `v0.5.0`, or `latest`).                                         |
| `ssh-private-key`   | no       | `""`                     | SSH private key contents for rsync/SSH deploys.                                                       |
| `ssh-key-name`      | no       | `""`                     | Filename under `~/.ssh` for the key. Required when `ssh-private-key` is set.                          |
| `ssh-known-hosts`   | no       | `""`                     | Entries appended to `~/.ssh/known_hosts`.                                                             |
| `args`              | no       | `""`                     | Extra arguments appended to `smb deploy`.                                                             |
| `cli-repository`    | no       | `smbcloudXYZ/smbcloud-cli` | Repository to download the CLI binary from.                                                         |

## Outputs

| Output     | Description                              |
| ---------- | ---------------------------------------- |
| `smb-path` | Absolute path to the installed `smb` binary. |
| `version`  | Resolved CLI version that was installed. |

After this step runs, `smb` is on `PATH` and authenticated, so later steps can
call it directly (e.g. `smb me`, `smb migrate`).

## How it works

1. **Install.** Downloads the prebuilt `smb-<os>-<arch>` release binary for the
   runner, makes it executable, and adds it to `PATH`. Works on Linux, macOS, and
   Windows runners (amd64/arm64).
2. **Configure.** Writes the token to `~/.smb/token` (production) or
   `~/.smb-dev/token` (dev) with `600` perms, then installs the SSH key and
   `known_hosts` if you passed them.
3. **Deploy.** Runs `smb --ci --environment <env> deploy [--project <name>]` in
   `working-directory`. The exit code propagates, so a failed deploy fails the job.

## Provisioning the token

Logging in is interactive and cannot run under `--ci`, so provision the token
ahead of time and store its contents in a CI secret. See
[`docs/ci.md`](https://github.com/smbcloudXYZ/smbcloud-cli/blob/main/docs/ci.md)
in the CLI repo for token setup and the full non-interactive behavior reference.

## License

[Apache-2.0](LICENSE)

---
name: Bug report
about: Create a report to help us improve
title: ''
labels: bug
assignees: ''

---

**Describe the bug**
A clear and concise description of what the bug is.

**Workflow snippet**
A minimal `uses:` step (and any surrounding job config) that reproduces the problem. Redact secrets.

```yaml
- uses: smbcloudXYZ/smbcloud-deploy-action@v1
  with:
    token: ${{ secrets.SMB_TOKEN }}
    # ...
```

**To Reproduce**
Steps to reproduce the behavior.

**Expected behavior**
A clear and concise description of what you expected to happen.

**Logs**
Relevant job log output (with the action's step expanded). Redact secrets, tokens, and hostnames.

**Environment**
 - Runner OS/arch: [e.g. ubuntu-latest / x86_64]
 - Action version (tag or SHA): [e.g. v1.0.0]
 - Resolved CLI version: [from the action's `version` output, or `smb --version`]
 - smbCloud environment: [production / dev]
 - Deploy kind: [nextjs-ssr / vite-spa / rails / rust / swift / rsync / git]

**Additional context**
Add any other context about the problem here.

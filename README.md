# CodeCordon Security Gate

Add a deterministic security check for AI-generated and vibe-coded applications to any GitHub Actions workflow.

CodeCordon reports exact files, lines, and suggested fixes for known patterns involving hardcoded secrets, missing authentication, injection, unsafe LLM usage, insecure configuration, and web security mistakes. A passing result means no configured known-pattern gate failed; it is **not** a security certification or penetration test.

## Try the scanner first

Scan any public GitHub repository without an account:

**[Run a free public-repository scan](https://codecordon.up.railway.app/?utm_source=github&utm_medium=action_repo&utm_campaign=public_action)**

See what honest launch evidence looks like:

**[Inspect the FieldBase ShipBond example](https://codecordon.up.railway.app/shipbond/example?utm_source=github&utm_medium=action_repo&utm_campaign=public_action)**

## Add the pull-request gate

1. Create a [CodeCordon account](https://codecordon.up.railway.app/register?utm_source=github&utm_medium=action_repo&utm_campaign=action_install), upgrade to Pro, and create an API key in Settings.
2. Store the key in your repository as an Actions secret named `CODECORDON_API_KEY`. Never commit it to a workflow.
3. Add `.github/workflows/codecordon.yml`:

```yaml
name: CodeCordon security scan

on:
  pull_request:
  workflow_dispatch:

permissions:
  contents: read

jobs:
  codecordon:
    runs-on: ubuntu-latest
    timeout-minutes: 10
    steps:
      - uses: actions/checkout@v4
      - uses: actions/setup-node@v4
        with:
          node-version: 22
      - uses: fj8b85t9g6-blip/codecordon-action@v1
        with:
          api-key: ${{ secrets.CODECORDON_API_KEY }}
          fail-on: critical
          min-score: 60
```

Pin the action to a reviewed commit SHA instead of `@v1` when your supply-chain policy requires immutable references.

## Inputs

| Input | Required | Default | Meaning |
| --- | --- | --- | --- |
| `api-key` | Yes | — | Pro API key supplied through a GitHub Actions secret |
| `target` | No | `.` | Checked-out directory to scan |
| `fail-on` | No | `critical` | Lowest severity that fails the job |
| `min-score` | No | `60` | Minimum passing score from 0–100 |

Exit code `0` means the configured gate passed, `1` means the scan ran but failed the gate, and `2` means the scan could not run.

## Client-launch evidence for studios

CodeCordon's ShipBond binds an exact completed scan to independent HTTPS deployment checks and publishes the result as a seven-day evidence record. Studios can [prepay 30 client-launch activations for $1,250](https://codecordon.up.railway.app/shipbond?utm_source=github&utm_medium=action_repo&utm_campaign=studio_pack) or activate one launch for $49.

ShipBond is point-in-time evidence, not a certification, warranty, compliance statement, accessibility audit, or penetration test.

## Data handling

The action excludes dependencies, build output, lockfiles, binaries, symlinks, and files larger than 512 KB before uploading an in-memory archive to CodeCordon. Review the current [Security and Data Handling notice](https://codecordon.up.railway.app/security) before using it on a repository.

## License

The action wrapper is available under the MIT License. Use of the hosted CodeCordon service is governed by its [Terms](https://codecordon.up.railway.app/terms) and [Privacy Notice](https://codecordon.up.railway.app/privacy).

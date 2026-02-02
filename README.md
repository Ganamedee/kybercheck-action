# KyberCheck GitHub Action

Scan your codebase for quantum-vulnerable cryptography. Prepare for the post-quantum era.

## Usage

Add this workflow to your repository at `.github/workflows/kybercheck.yml`:

```yaml
name: KyberCheck Scan
on:
  push:
    branches: [main, master]
  pull_request:
  workflow_dispatch:
    inputs:
      scan_id:
        required: false
      api_url:
        required: false

jobs:
  scan:
    runs-on: ubuntu-latest
    steps:
      - uses: actions/checkout@v4
      - uses: Ganamedee/kybercheck-action@v1
        with:
          api-key: ${{ secrets.KYBERCHECK_API_KEY }}
          scan-id: ${{ inputs.scan_id }}
          api-url: ${{ inputs.api_url }}
```

## Inputs

| Input | Description | Required | Default |
|-------|-------------|----------|---------|
| `api-key` | Your KyberCheck API Key | Yes | - |
| `scan-id` | Scan ID (auto-filled from dashboard) | No | - |
| `api-url` | API URL for results | No | `https://kybercheck.com` |
| `path` | Path to scan | No | `.` |
| `fail-on-critical` | Fail build on critical vulnerabilities | No | `false` |

## Getting an API Key

1. Sign up at [kybercheck.com](https://kybercheck.com)
2. Go to Settings → API Keys
3. Create a new API key
4. Add it as a GitHub secret named `KYBERCHECK_API_KEY`

## Dashboard Integration

When you trigger scans from the KyberCheck dashboard, results automatically appear there. The `scan-id` and `api-url` inputs enable this integration.

## Learn More

- [Documentation](https://kybercheck.com/docs)
- [GitHub Actions Integration Guide](https://kybercheck.com/docs/github-actions)

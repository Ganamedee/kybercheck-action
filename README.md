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
| `exclude` | Patterns to ignore (comma-separated globs) | No | - |
| `languages` | Only scan these languages (comma-separated) | No | all |
| `fail-on-critical` | Fail build on critical vulnerabilities | No | `false` |

## Ignoring Files

Use the `exclude` input to skip certain files or directories. Patterns use glob syntax:

```yaml
- uses: Ganamedee/kybercheck-action@v1
  with:
    api-key: ${{ secrets.KYBERCHECK_API_KEY }}
    exclude: "**/test/**, **/*.test.js, examples/**"
```

### Common Patterns

| Pattern | What it ignores |
|---------|-----------------|
| `**/test/**` | All `test` directories |
| `**/*.test.js` | All JavaScript test files |
| `**/*_test.go` | All Go test files |
| `examples/**` | The `examples` directory |
| `legacy/**` | The `legacy` directory |

## Filtering Languages

Only scan specific languages using the `languages` input:

```yaml
- uses: Ganamedee/kybercheck-action@v1
  with:
    api-key: ${{ secrets.KYBERCHECK_API_KEY }}
    languages: "python,javascript,rust"
```

## Blocking Failed Builds

To prevent merging insecure code, you can fail the CI build if vulnerabilities are detected:

```yaml
- uses: Ganamedee/kybercheck-action@v1
  with:
    api-key: ${{ secrets.KYBERCHECK_API_KEY }}
    fail-on-critical: true
```
> **Note:** This stops the build if *any* vulnerability is found (not just critical ones). To customize this, use a `.kybercheck.toml` config file.

```

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


# PreviewLocker Demo

This repository demonstrates PreviewLocker running inside a GitHub pull request.

PreviewLocker can:

- create locked, expiring preview links
- scan preview URLs for common exposure risks
- post a security report directly on pull requests
- optionally fail the workflow when warnings are found

## Demo preview page

This repository can also serve a static GitHub Pages demo preview page from
`index.html`. It is designed as a simple PreviewLocker demonstration target and
includes `assets/app.js` plus an intentionally exposed `assets/app.js.map` file
so GitHub Pages can host a realistic static preview for scanning and reporting.

## Demo workflow

```yaml
name: PreviewLocker Demo

on:
  pull_request:
    types: [opened, synchronize, reopened]

permissions:
  contents: read
  issues: write
  pull-requests: write

jobs:
  previewlocker:
    runs-on: ubuntu-latest

    steps:
      - name: Lock, scan, and comment on preview
        uses: ModelGuardHQ-Tools/preview-locker-action@main
        with:
          api_key: ${{ secrets.PREVIEW_LOCKER_API_KEY }}
          preview_url: https://example.com
          expires_in: 3600
          comment_on_pr: true
          scan_preview: true
          fail_on_risk: false
          github_token: ${{ secrets.GITHUB_TOKEN }}

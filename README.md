# Assign Copilot Review Action

GitHub Action to automatically assign GitHub Copilot as a reviewer on pull requests.

## Features

- Automatically assigns GitHub Copilot as a reviewer using the correct bot username (`copilot-pull-request-reviewer[bot]`)
- Simple shell-based implementation using `gh api` command
- Better error handling with `gh` CLI
- Uses `GH_TOKEN` or `GITHUB_TOKEN` for authentication
- No dependencies required (composite action, `gh` CLI is pre-installed on GitHub Actions runners)

## Requirements

- Repository must have GitHub Copilot code review enabled
- Personal Access Token (PAT) with `pull_requests: write` permission is required
  - `GITHUB_TOKEN` may not work in some cases due to permission limitations
  - It is recommended to use a PAT stored in repository secrets

## Usage

### Basic Usage (Recommended)

Use a Personal Access Token (PAT) for reliable operation:

```yaml
name: Assign Copilot Review

on:
  pull_request:
    types: [opened, synchronize, reopened]

jobs:
  assign-copilot:
    runs-on: ubuntu-latest
    steps:
      - name: Assign Copilot as reviewer
        uses: mokmok-dev/assign-copilot-review-action@v1
        with:
          github-token: ${{ secrets.GH_TOKEN }}
          pr-number: ${{ github.event.pull_request.number }}
```

**Note**: Create a PAT with `pull_requests: write` permission and add it to your repository secrets as `GH_TOKEN`.

### Using GITHUB_TOKEN (Not Recommended)

`GITHUB_TOKEN` may not work due to permission limitations:

```yaml
name: Assign Copilot Review

on:
  pull_request:
    types: [opened, synchronize, reopened]

permissions:
  pull-requests: write

jobs:
  assign-copilot:
    runs-on: ubuntu-latest
    steps:
      - name: Assign Copilot as reviewer
        uses: mokmok-dev/assign-copilot-review-action@v1
        with:
          github-token: ${{ secrets.GITHUB_TOKEN }}
          pr-number: ${{ github.event.pull_request.number }}
```

### Assign to a Different Repository

You can specify a different repository:

```yaml
- name: Assign Copilot as reviewer to another repo
  uses: mokmok-dev/assign-copilot-review-action@v1
  with:
    github-token: ${{ secrets.GH_TOKEN }}
    pr-number: 123
    repository: owner/repo
```

**Note**: The token must have `pull_requests: write` permission for the target repository.

## Inputs

| Input | Description | Required | Default |
|-------|-------------|----------|---------|
| `github-token` | GitHub token with pull request write permissions | Yes | - |
| `pr-number` | Pull request number | Yes | - |
| `repository` | Repository in the format owner/repo | No | `${{ github.repository }}` |

## Permissions

The action requires the following permissions:

```yaml
permissions:
  pull-requests: write
```

## License

MIT

## References

- [ChrisCarini/gh-copilot-review](https://github.com/ChrisCarini/gh-copilot-review) - Inspiration for Copilot review automation
- [mokmok-dev/delete-actions-cache](https://github.com/mokmok-dev/delete-actions-cache) - GitHub Actions implementation pattern reference

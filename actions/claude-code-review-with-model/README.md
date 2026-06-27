# Claude Code Review

A reusable composite action to automatically run Claude Code Review on a pull request.

## Inputs

| Name                      | Description                                                  | Required | Default |
|---------------------------|--------------------------------------------------------------|----------|---------|
| `claude_code_oauth_token` | OAuth token for Claude Code                                  | No       | N/A     |
| `anthropic_api_key`       | Anthropic API key for Claude Code                            | No       | N/A     |
| `claude_model`            | Explicitly requested Claude model (full name or alias)       | No       | N/A     |
| `verify_author_write`     | If true, verifies the author has write access.               | No       | true    |

## Usage Example

```yaml
name: Claude Code Review
on:
  pull_request:
    types: [opened, synchronize]

jobs:
  claude-review:
    runs-on: ubuntu-latest
    permissions:
      contents: read
      pull-requests: read
      issues: read
      id-token: write
    steps:
      - name: Code Review
        uses: your-org/github-actions/actions/claude-code-review-with-model@main
        with:
          anthropic_api_key: ${{ secrets.ANTHROPIC_API_KEY }}
          # claude_code_oauth_token: ${{ secrets.CLAUDE_CODE_OAUTH_TOKEN }}
          # claude_model: 'sonnet'
          verify_author_write: 'true'
```

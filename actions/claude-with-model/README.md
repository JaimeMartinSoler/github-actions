# Claude Code with Model

A reusable composite action to interact with Claude Code via issues and PR comments. It supports determining the requested model dynamically from the issue/PR trigger text.

## Inputs

| Name                      | Description                                                  | Required | Default |
|---------------------------|--------------------------------------------------------------|----------|---------|
| `claude_code_oauth_token` | OAuth token for Claude Code                                  | No       | N/A     |
| `anthropic_api_key`       | Anthropic API key for Claude Code                            | No       | N/A     |
| `claude_model`            | Explicitly requested Claude model (full name or alias)       | No       | N/A     |
| `verify_author_write`     | If true, verifies the author has write access.               | No       | true    |
| `base_branch`             | Base branch for checkout and PRs                             | No       | develop |

## Usage Example

```yaml
name: Claude Interactive
on:
  issue_comment:
    types: [created]
  pull_request_review_comment:
    types: [created]

jobs:
  claude:
    if: |
      (github.event_name == 'issue_comment' && contains(github.event.comment.body, '@claude')) ||
      (github.event_name == 'pull_request_review_comment' && contains(github.event.comment.body, '@claude'))
    runs-on: ubuntu-latest
    permissions:
      contents: read
      pull-requests: write
      issues: write
      id-token: write
      actions: read
    steps:
      - name: Run Claude
        uses: JaimeMartinSoler/github-actions/actions/claude-with-model@main
        with:
          anthropic_api_key: ${{ secrets.ANTHROPIC_API_KEY }}
          # claude_code_oauth_token: ${{ secrets.CLAUDE_CODE_OAUTH_TOKEN }}
          # claude_model: 'sonnet'
          verify_author_write: 'true'
          # base_branch: 'develop'
```

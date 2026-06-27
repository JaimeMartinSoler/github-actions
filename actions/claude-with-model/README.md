# Claude Code with Model

A reusable composite action to interact with Claude Code via issues and PR comments. It supports determining the requested model dynamically from the issue/PR trigger text.

## Inputs

| Name                      | Description                                                  | Required | Default |
|---------------------------|--------------------------------------------------------------|----------|---------|
| `claude_code_oauth_token` | OAuth token for Claude Code                                  | No       | N/A     |
| `anthropic_api_key`       | Anthropic API key for Claude Code                            | No       | N/A     |
| `claude_model`            | Explicitly requested Claude model (full name or alias)       | No       | N/A     |
| `verify_author_write`     | If true, verifies the author has write access.               | No       | true    |
| `skip_if_bot`             | If true, skips execution gracefully when triggered by a bot. | No       | true    |
| `base_branch`             | Base branch for checkout and PRs                             | No       | develop |

## Usage Example

This example is ready to copy-paste into your `claude.yml` workflow file.

```yaml
name: Claude Code

on:
  issue_comment:
    types: [created]
  pull_request_review_comment:
    types: [created]
  issues:
    types: [opened, assigned]
  pull_request_review:
    types: [submitted]

jobs:
  claude:
    if: |
      (github.event_name == 'issue_comment' && contains(github.event.comment.body, '@claude')) ||
      (github.event_name == 'pull_request_review_comment' && contains(github.event.comment.body, '@claude')) ||
      (github.event_name == 'pull_request_review' && contains(github.event.review.body, '@claude')) ||
      (github.event_name == 'issues' && (contains(github.event.issue.body, '@claude') || contains(github.event.issue.title, '@claude')))
    runs-on: ubuntu-latest
    permissions:
      contents: read
      pull-requests: write # Required to post the "no write access" comment
      issues: write # Required to post the "no write access" comment
      id-token: write
      actions: read # Required for Claude to read CI results on PRs
    steps:
      - name: claude-with-model
        uses: JaimeMartinSoler/github-actions/actions/claude-with-model@main
        with:
          # --- IMPLEMENTER ACTION SHOULD ONLY MODIFY THESE PARAMETERS: ---
          # anthropic_api_key: ${{ secrets.ANTHROPIC_API_KEY }} # (provide either this or claude_code_oauth_token)
          claude_code_oauth_token: ${{ secrets.CLAUDE_CODE_OAUTH_TOKEN }} # (provide either this or anthropic_api_key)
          # claude_model: 'opus' # (default: empty, get from event body text like 'claude model opus/sonnet/haiku')
          verify_author_write: 'true' # (default: 'true')
          skip_if_bot: 'true' # (default: 'true')
          base_branch: 'develop' # (default: 'develop')
```

# Claude Code Review

A reusable composite action to automatically run Claude Code Review on a pull request.

## Inputs

| Name                      | Description                                                  | Required | Default |
|---------------------------|--------------------------------------------------------------|----------|---------|
| `claude_code_oauth_token` | OAuth token for Claude Code                                  | No       | N/A     |
| `anthropic_api_key`       | Anthropic API key for Claude Code                            | No       | N/A     |
| `claude_model`            | Explicitly requested Claude model (full name or alias)       | No       | N/A     |
| `verify_author_write`     | If true, verifies the author has write access.               | No       | true    |
| `skip_if_bot`             | If true, skips execution gracefully when triggered by a bot. | No       | true    |

## Usage Example

This example is ready to copy-paste into your `claude-code-review.yml` workflow file.

```yaml
name: Claude Code Review

on:
  pull_request:
    types: [opened, synchronize]
    # Optional: Only run on specific file changes
    # paths:
    #   - "src/**/*.ts"
    #   - "src/**/*.tsx"

jobs:
  claude-review:
    runs-on: ubuntu-latest
    permissions:
      contents: read
      pull-requests: read
      issues: read
      id-token: write

    steps:
      - name: claude-code-review-with-model
        uses: JaimeMartinSoler/github-actions/actions/claude-code-review-with-model@main
        with:
          # --- IMPLEMENTER ACTION SHOULD ONLY MODIFY THESE PARAMETERS: ---
          # anthropic_api_key: ${{ secrets.ANTHROPIC_API_KEY }} # (provide either this or claude_code_oauth_token)
          claude_code_oauth_token: ${{ secrets.CLAUDE_CODE_OAUTH_TOKEN }} # (provide either this or anthropic_api_key)
          claude_model: 'opus' # (default: empty, get from event body text like 'claude model opus/sonnet/haiku', but review has no event body for this, so provide this parameter)
          verify_author_write: 'true' # (default: 'true')
          skip_if_bot: 'true' # (default: 'true')
```

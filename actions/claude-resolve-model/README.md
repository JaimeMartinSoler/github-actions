# Claude Resolve Model

A reusable composite action to check if the user is authorized and resolve which Claude model to use, either via explicit input or by parsing an issue/PR comment.

## Inputs

| Name                      | Description                                                  | Required | Default |
|---------------------------|--------------------------------------------------------------|----------|---------|
| `claude_code_oauth_token` | OAuth token for Claude Code                                  | No       | N/A     |
| `anthropic_api_key`       | Anthropic API key for Claude Code                            | No       | N/A     |
| `claude_model`            | Explicitly requested Claude model (full name or alias)       | No       | N/A     |
| `verify_author_write`     | If true, verifies the author has write access.               | No       | true    |

## Outputs

| Name         | Description                                        |
|--------------|----------------------------------------------------|
| `authorized` | Boolean ('true'/'false') indicating authorization  |
| `model`      | The resolved Claude model string, or empty string  |

## Usage Example

```yaml
steps:
  - name: Resolve Model
    id: resolve
    uses: JaimeMartinSoler/github-actions/actions/claude-resolve-model@main
    with:
      claude_model: 'sonnet'
      anthropic_api_key: ${{ secrets.ANTHROPIC_API_KEY }}

  - name: Run logic
    if: steps.resolve.outputs.authorized == 'true'
    run: echo "Resolved to ${{ steps.resolve.outputs.model }}"
```

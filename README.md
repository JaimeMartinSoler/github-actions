# Reusable GitHub Actions

This repository contains a collection of reusable GitHub Actions for use across our various repositories. Centralizing these actions helps us avoid duplication, standardize workflows, and maintain CI/CD logic efficiently.

## Available Actions

### [claude-with-model](actions/claude-with-model/)

Runs Claude Code interactively from issue and PR comments. Supports dynamic model selection (via comment text or explicit input), write-access gating, and configurable base branches.

### [claude-code-review-with-model](actions/claude-code-review-with-model/)

Automatically runs a Claude Code Review on pull requests, providing feedback on code quality, bugs, performance, and security. Supports model selection.

### [claude-resolve-model](actions/claude-resolve-model/)

Internal building block used by the other actions. Handles author authorization checks and resolves which Claude model to use from explicit input or issue/PR trigger text.

## Directory Structure

- `actions/`: Contains all reusable actions, each in its own subfolder.
  - `actions/<action-name>/`: Individual action folder.
    - `action.yml`: The action definition file.
    - `README.md`: Documentation for how to use this specific action.

## How to Use

To use an action from this repository in another workflow, reference it using the repository name, path to the action, and a version tag or branch:

```yaml
jobs:
  claude:
    runs-on: ubuntu-latest
    permissions:
      contents: read
      pull-requests: write
      issues: write
      id-token: write
      actions: read
    steps:
      - name: Run Claude
        uses: your-org/github-actions/actions/claude-with-model@main
        with:
          anthropic_api_key: ${{ secrets.ANTHROPIC_API_KEY }}
```

See each action's `README.md` for full input/output documentation and usage examples.

## Contributing

1. Create a new folder under `actions/` for your action.
2. Add an `action.yml` defining the inputs, outputs, and runs steps.
3. Add a `README.md` documenting its usage.
4. Update this main `README.md` if necessary.

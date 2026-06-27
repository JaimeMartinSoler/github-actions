# Claude Standards and Guidelines

This file outlines the standard commands, code style, and architectural guidelines that Claude (or any AI assistant) should follow when working in this repository.

## Commands
Since this repository primarily consists of YAML files for GitHub Actions, build and test commands are minimal. However, we ensure standard linting for YAML and markdown.

- **Lint YAML**: `npx yaml-lint actions/**/*.yml`
- **Lint Markdown**: `npx markdownlint-cli "**/*.md"`

## Code Style
- **YAML Formatting**:
  - Use 2 spaces for indentation.
  - Keys should be lowercase and hyphenated where appropriate (e.g., `runs-on`, `input-name`).
  - Quote string values if they contain special characters.
- **Naming Conventions**:
  - Action folders should be `kebab-case` (e.g., `setup-node-env`, `build-docker-image`).
  - Inputs and outputs in `action.yml` should use `snake_case` consistently (matching `anthropics/claude-code-action` conventions).

## Architecture
- **Modularity**: Each action must be self-contained within its own directory under `actions/`.
- **Documentation**: Every action directory must include a `README.md` detailing its inputs, outputs, and providing a usage example.
- **Simplicity**: Prefer composite actions for simple orchestration of existing steps. Use JavaScript or Docker actions only when complex logic is strictly necessary.
- **Versioning**: When referencing other actions within composite actions, pin to specific major tags (e.g., `@v4`) rather than `@main` or `@master` to ensure stability.

Follow these guidelines diligently to keep our reusable actions clean and maintainable.

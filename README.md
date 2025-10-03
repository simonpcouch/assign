# assign-claude-code

A reusable GitHub workflow that runs Claude Code in an R package directory with R, package dependencies, and an MCP server to read R package documentation preconfigured.

## Usage

Create a workflow file in your repository at `.github/workflows/assign-claude-code.yml`:

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
    uses: simonpcouch/assign/.github/workflows/assign-claude-code.yml@main
    secrets:
      ANTHROPIC_API_KEY: ${{ secrets.ANTHROPIC_API_KEY }}
```

Add your Anthropic API key as a repository secret named `ANTHROPIC_API_KEY`.

Tag Claude in issues, pull requests, or comments with `@claude` to invoke the action.

## What it does

- Checks out your repository
- Installs R and package dependencies (including btw from posit-dev/btw)
- Configures a [btw MCP server](https://posit-dev.github.io/btw/) with documentation tools
- Runs Claude Code with access to R package documentation

## MCP Configuration

The action automatically configures a MCP server with [btw](https://posit-dev.github.io/btw/) using:
```r
btw::btw_mcp_server(tools = btw::btw_tools(tools = "docs"))
```

This gives Claude access to R package documentation during execution.

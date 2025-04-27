# MCP Discovery Action

A GitHub Action to run the [MCP Discovery](https://github.com/rust-mcp-stack/mcp-discovery) CLI for creating, updating or printing MCP server capability details in a Markdown or other formatted file.

## Overview

This action runs the [MCP Discovery](https://github.com/rust-mcp-stack/mcp-discovery) CLI to discover MCP server capabilities and create/update a file with MCP server details (`create` or `update` commands). It supports all CLI options, including built-in templates (`md`, `md-plain`, `html`, `txt`), custom Handlebars template files, or template strings.

The action downloads the latest version of the CLI binary by default, but you can specify a specific version using the action's version input.

## Inputs

| Input                | Description                                                                                      | Required | Default               |
| -------------------- | ------------------------------------------------------------------------------------------------ | -------- | --------------------- |
| `command`            | CLI command to run (`create`, `update` or `print`).                                              | Yes      | -                     |
| `mcp-launch-command` | Command and arguments to launch the MCP server (e.g., `java -jar mcp.jar`).                      | Yes      | -                     |
| `filename`           | Output file path for `create`/`update` commands (e.g., `docs/mcp-details.md`).                   | No       | `mcp-discovery.md`    |
| `template`           | Built-in template for output (`md`, `md-plain`, `html`, `txt`).                                  | No       | -                     |
| `template-file`      | Path to a custom Handlebars template file in the repository.                                     | No       | -                     |
| `template-string`    | Custom Handlebars template content as a string.                                                  | No       | -                     |
| `token`              | Optional GitHub token with content write permission.                                             | No       | `${{ github.token }}` |
| `version`            | Version of the `mcp-discovery` CLI to use (e.g., `v0.1.2`). Use `latest` for the latest release. | No       | `latest`              |

💡 For more information and command examples, refer to the [MCP Discovery documentation](https://rust-mcp-stack.github.io/mcp-discovery).

## Usage

### Example 1: Create a Markdown File

Create `mcp-discovery.md` with MCP server capabilities and commit it if changed.

```yaml
name: MCP Discovery (create)
on:
  push:
    branches:
      - main
permissions:
  contents: write
jobs:
  generate-md:
    runs-on: ubuntu-latest
    steps:
      - uses: actions/checkout@v4
        with:
          token: ${{ secrets.GITHUB_TOKEN }}

      - uses: rust-mcp-stack/mcp-discovery-action@v1
        with:
          version: 'latest'
          command: 'create'
          mcp-launch-command: '[THE COMMAND YOU USE TO START YOUR MCP SERVER]'
          filename: 'mcp-discovery.md'
```

### Example 2: Update a designated section of the README.md file

💡 For more information about `update` command and template markers, visit [MCP Discovery documentation](https://rust-mcp-stack.github.io/mcp-discovery/#/guide/mcp-discovery-markers).

```yaml
name: MCP Discovery (update)
on:
  push:
    branches:
      - main
permissions:
  contents: write
jobs:
  generate-md:
    runs-on: ubuntu-latest
    steps:
      - uses: actions/checkout@v4
        with:
          token: ${{ secrets.GITHUB_TOKEN }}

      - uses: rust-mcp-stack/mcp-discovery-action@v1
        with:
          version: 'latest'
          command: 'update'
          mcp-launch-command: '[THE COMMAND YOU USE TO START YOUR MCP SERVER]'
          filename: 'README.md'
```

## Contributing

Contributions are welcome! Please open an issue or pull request in the [mcp-discovery-action repository](https://github.com/rust-mcp-stack/mcp-discovery).

## License

MIT License. See [LICENSE](LICENSE) for details.

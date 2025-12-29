# gh-project-ext

A GitHub CLI extension for managing GitHub Projects V2 Kanban boards.

## Features

- **Board View**: ASCII Kanban board with Status columns and Priority swimlanes
- **Ready Queue**: See items ready to work on, sorted by priority
- **Quick Move**: Move items between status columns by name
- **Claim**: Assign an issue to yourself and move to In Progress in one command
- **Smart Filtering**: Default to top-level items (no parent) and current repository

## Installation

```bash
gh extension install jwilger/gh-project-ext
```

### Prerequisites

- [GitHub CLI](https://cli.github.com/) 2.0+
- `project` scope for your token: `gh auth refresh -s project`
- Optionally: [gh-issue-ext](https://github.com/jwilger/gh-issue-ext) for linked branch creation

## Setup

Run the interactive setup to configure your default project:

```bash
gh project-ext setup
```

This creates a `.github-project` config file:

```yaml
owner: jwilger
project: 11
```

## Commands

### ready

List items in "Ready" status, sorted by priority:

```bash
gh project-ext ready              # Current repo, top-level items
gh project-ext ready --all        # Include sub-issues
gh project-ext ready --all-repos  # All repositories
gh project-ext ready --json       # JSON output
```

### board

Show a text-based Kanban board:

```bash
gh project-ext board              # Current repo, top-level items
gh project-ext board --all        # Include sub-issues
```

Output:
```
[Backlog] (5)
  P0:
    #42: Critical bug fix
  P1:
    #43: Important feature

[Ready] (3)
  P0:
    #44: High priority item
...
```

### move

Move an issue to a different status:

```bash
gh project-ext move 42 "In progress"
gh project-ext move 42 "Done"
```

Valid statuses: `Backlog`, `Ready`, `In progress`, `In review`, `Done`

### claim

Assign an issue to yourself and move to In Progress:

```bash
gh project-ext claim 42
```

This will:
1. Move the issue to "In Progress"
2. Assign you to the issue
3. Optionally create a linked development branch (prompts you)

### show

Display project structure and fields:

```bash
gh project-ext show
gh project-ext show --json
```

## Global Options

| Option | Description |
|--------|-------------|
| `--owner <login>` | Project owner (user or org) |
| `--project <number>` | Project number |
| `--all` | Include sub-issues (default: top-level only) |
| `--all-repos` | Include all repositories (default: current repo) |
| `--json` | Output as JSON |

## Configuration

The extension looks for configuration in this order:

1. `.github-project` in current directory or git root
2. `~/.config/gh-project-ext/config`

Command-line flags (`--owner`, `--project`) override config file settings.

## Integration with gh-issue-ext

If [gh-issue-ext](https://github.com/jwilger/gh-issue-ext) is installed, the `claim` command will use it to create linked branches. Otherwise, it falls back to `gh issue develop`.

## License

MIT

# dda - Dependabot Alerts Viewer

An interactive command-line tool for browsing and viewing GitHub Dependabot security alerts across your organization's repositories.

## Features

- **Organization-wide view** - See all open Dependabot alerts across your GitHub organization
- **Interactive browsing** - Use fuzzy search to filter repositories and alerts
- **Severity highlighting** - Alerts are color-coded and sorted by severity (Critical > High > Medium > Low)
- **Clipboard integration** - Selected alert details are automatically copied to your clipboard in Markdown format
- **Non-interactive mode** - Script-friendly output for automation and piping

## Installation

1. Clone this repository:
   ```bash
   git clone https://github.com/your-org/security-alerts.git
   cd security-alerts
   ```

2. Make the script executable (if not already):
   ```bash
   chmod +x dda
   ```

3. Install dependencies:
   ```bash
   brew install gh fzf jq
   ```

4. Authenticate with GitHub CLI:
   ```bash
   gh auth login
   ```

## Configuration

Set the `GITHUB_ORG` environment variable to your GitHub organization name:

```bash
export GITHUB_ORG=your-organization
```

You may want to add this to your shell profile (`~/.bashrc`, `~/.zshrc`, etc.):

```bash
echo 'export GITHUB_ORG=your-organization' >> ~/.zshrc
```

## Usage

### Interactive Mode (default)

Simply run the script to interactively browse alerts:

```bash
./dda
```

This will:
1. Fetch all open Dependabot alerts for your organization
2. Display a filterable list of repositories with alert counts
3. Let you select a repository to view its alerts
4. Let you select an alert to view details (automatically copied to clipboard)

### Non-Interactive Modes

**List all alerts for a specific repository:**
```bash
./dda --repo my-service
```

**Get details for a specific alert:**
```bash
./dda --repo my-service --alert 42
```

**Copy alert details to clipboard:**
```bash
./dda -r my-service -a 42 | pbcopy
```

### Options

| Option | Description |
|--------|-------------|
| `-h, --help` | Show help message |
| `-r, --repo REPO` | Specify repository name (skips repo selection) |
| `-a, --alert NUMBER` | Specify alert number (requires `--repo`) |

## Output Format

Alert details are output in Markdown format, including:

- Package name and ecosystem
- Severity level and CVE/GHSA identifiers
- Vulnerability description
- Affected versions
- Fix information (if available)
- Links to the alert and advisory

### Example Output

```markdown
# Dependabot Alert: my-service #42

| Field | Value |
|-------|-------|
| **Package** | `lodash` |
| **Ecosystem** | npm |
| **Severity** | **HIGH** |
| **CVE** | CVE-2021-23337 |
| **GHSA** | GHSA-35jh-r3h4-6jhm |
| **State** | open |
| **Created** | 2024-01-15T10:30:00Z |

## Vulnerability

Command Injection in lodash

...

## Fix

Upgrade to version `4.17.21` or later

## Links

- [Alert](https://github.com/your-org/my-service/security/dependabot/42)
- [Advisory](https://github.com/advisories/GHSA-35jh-r3h4-6jhm)
```

## Dependencies

| Tool | Purpose | Installation |
|------|---------|--------------|
| [gh](https://cli.github.com/) | GitHub CLI for API access | `brew install gh` |
| [fzf](https://github.com/junegunn/fzf) | Fuzzy finder for interactive selection | `brew install fzf` |
| [jq](https://jqlang.github.io/jq/) | JSON processing | `brew install jq` |

> **Note:** `fzf` is only required for interactive mode. Non-interactive modes (`--repo` and `--alert` flags) work without it.

## Keyboard Shortcuts (Interactive Mode)

| Key | Action |
|-----|--------|
| `Enter` | Select item |
| `Ctrl+C` | Cancel/Exit |
| `↑/↓` | Navigate list |
| `Type` | Filter results |

## Requirements

- macOS or Linux
- Bash 4.0+
- GitHub CLI authenticated with access to your organization's security alerts
- Appropriate GitHub permissions to view Dependabot alerts

## License

MIT

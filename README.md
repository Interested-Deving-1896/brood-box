[update-readmes]   Mode: rewrite — migrating to template structure...
# brood-box

[![Built with Ona](https://ona.com/build-with-ona.svg)](https://app.ona.com/#https://github.com/Interested-Deving-1896/brood-box)

<!-- AI:start:what-it-does -->
_Description pending._
<!-- AI:end:what-it-does -->

## Architecture

<!-- AI:start:architecture -->
_Architecture documentation pending._
<!-- AI:end:architecture -->

## Install

<!-- Add installation instructions here. This section is yours — the AI will not modify it. -->

```bash
git clone https://github.com/Interested-Deving-1896/brood-box.git
cd brood-box
```

## Usage


```bash
# Run with a specific agent
bbox claude-code
bbox codex
bbox opencode
bbox gemini

# Override resources
bbox claude-code --cpus 4 --memory 4096

# Use a different workspace
bbox claude-code --workspace /path/to/project

# Enable interactive per-file review (snapshot mode only)
bbox claude-code --review

# Exclude files from snapshot
bbox claude-code --exclude "*.log" --exclude "tmp/"

# Skip snapshot isolation entirely: the agent writes directly to your workspace
# (no review, no undo). --yes is required on the first run.
bbox claude-code --workspace-mode=direct --yes

# Lock down egress to LLM provider only
bbox claude-code --egress-profile locked

# Allow additional egress hosts (DNS hostnames only, no IP addresses)
bbox claude-code --allow-host "internal-api.example.com:443"

# Disable MCP proxy
bbox claude-code --no-mcp

# Disable firmware download (use system libkrunfw only)
bbox claude-code --no-firmware-download

# Use a specific ToolHive group for MCP servers
bbox claude-code --mcp-group "coding-tools"

# Pass agent-specific arguments (after --)
bbox claude-code -- --help

# List available agents
bbox list
```

### Workspace modes

By default, bbox runs the agent against a copy-on-write snapshot of your workspace
and flushes changes back when the agent exits. No write lands on your real files
without going through the diff engine. Add `--review` to approve each file
interactively.

For quick, trusted edits where you're driving the agent turn-by-turn and snapshot
overhead isn't worth it, pass `--workspace-mode=direct`. The VM mounts your
workspace read-write and writes land immediately. In direct mode, `--review` and
`--exclude` are rejected (they only apply to snapshots), and git credential
sanitization is skipped. Per-workspace `.broodbox.yaml` cannot enable direct mode;
only the operator can, globally or on the CLI, and `--yes` is required on first
use. Use direct mode when you'd trust the agent with an unsandboxed shell anyway.
Otherwise stay on snapshot mode (the default).

## Configuration


Brood Box uses a three-level config system: CLI flags > per-workspace > global. CLI flags always win.

### Global config

`~/.config/broodbox/config.yaml`:

```yaml
defaults:
  cpus: 4
  memory: 4096
  egress_profile: "permissive"

workspace:
  mode: "snapshot"   # snapshot (default) or direct

review:
  enabled: true
  exclude_patterns:
    - "*.log"
    - "build/"

mcp:
  enabled: true
  group: "default"
  port: 4483
  session_ttl: "12h"   # idle eviction timeout for host MCP sessions

git:
  forward_token: true
  forward_ssh_agent: true

runtime:
  firmware_download: true

agents:
  claude-code:
    env_forward:
      - ANTHROPIC_API_KEY
      - CLAUDE_*
      - GITHUB_TOKEN
```

### Per-workspace config

`.broodbox.yaml` in your project root:

```yaml
defaults:
  cpus: 8
  memory: 8192

review:
  exclude_patterns:
    - "data/"
```

Note that `review.enabled` is **ignored** in per-workspace config for security.
An untrusted repo cannot disable review on your behalf.

`workspace.mode: direct` from per-workspace config is also ignored. An untrusted
repo cannot turn off snapshot isolation. Setting `workspace.mode: snapshot`
in `.broodbox.yaml` is allowed (tighten-only: a repo can force snapshot even if
the global config enables direct).

Similarly, `egress_profile` in per-workspace config cannot widen the global profile.

### Exclude patterns

`.broodboxignore` in your project root uses gitignore syntax:

```gitignore
# Exclude build artifacts
build/
dist/

# But include the config
!dist/config.json
```

Security-sensitive patterns (`.env*`, `*.pem`, `.ssh/`, `.aws/`, etc.) are **always excluded** and cannot be negated.

## CI

<!-- AI:start:ci -->
_CI documentation pending._
<!-- AI:end:ci -->

## Mirror chain

<!-- AI:start:mirror-chain -->
This repo is maintained in [`Interested-Deving-1896/brood-box`](https://github.com/Interested-Deving-1896/brood-box) and mirrored through:

```
Interested-Deving-1896/brood-box  ──►  OpenOS-Project-OSP/brood-box  ──►  OpenOS-Project-Ecosystem-OOC/brood-box
```

Changes flow downstream automatically via the hourly mirror chain in
[`fork-sync-all`](https://github.com/Interested-Deving-1896/fork-sync-all).
Direct commits to OSP or OOC are detected and opened as PRs back to `Interested-Deving-1896`.
<!-- AI:end:mirror-chain -->

## Contributors

<!-- AI:start:contributors -->
_Contributors pending._
<!-- AI:end:contributors -->

## Origins

<!-- AI:start:origins -->
_Original project — no upstream fork._
<!-- AI:end:origins -->

## Resources

<!-- AI:start:resources -->
_No additional resource files found._
<!-- AI:end:resources -->

## License

<!-- AI:start:license -->
[Apache-2.0](https://github.com/Interested-Deving-1896/brood-box/blob/main/LICENSE) © 2026 [Interested-Deving-1896](https://github.com/Interested-Deving-1896)
<!-- AI:end:license -->

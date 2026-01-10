# Symposium Agent Extension Packaging

This guide covers how to set up automated builds and releases for Symposium agent extensions.

## What This Workflow Does

The `package-agent-extension` workflow:

1. **Builds cross-platform binaries** - macOS (ARM + x64), Linux (ARM + x64), Windows (x64)
2. **Packages release artifacts** - tar.gz for Unix, zip for Windows
3. **Generates extension.json** - manifest for the ACP registry with binary name, args, and env vars

## Quick Links

- [Quick Start](./quickstart.md) - Get up and running in 5 minutes
- [Agent Guide](./agent-guide.md) - Copy-paste instructions for AI agents
- [GitHub Repository](https://github.com/symposium-dev/package-agent-extension)

## For AI Agents

If you're an AI agent setting up an extension, use the agent guide directly:

```
https://raw.githubusercontent.com/symposium-dev/package-agent-extension/main/AGENT-GUIDE.md
```

Fetch this URL and follow the instructions to set up automated builds and releases.

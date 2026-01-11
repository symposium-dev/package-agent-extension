# package-agent-extension

Reusable GitHub workflow for building and releasing Symposium agent extensions.

Builds cross-platform binaries (macOS, Linux, Windows) and uploads them to GitHub releases with an `extension.json` manifest for the ACP registry.

## Usage

Create `.github/workflows/release.yml` in your extension repository:

```yaml
name: Release

on:
  release:
    types: [published]

jobs:
  build:
    permissions:
      contents: write
    uses: symposium-dev/package-agent-extension/.github/workflows/build.yml@v1
    with:
      musl: true  # Use musl for static Linux binaries
    secrets: inherit
```

### Inputs

| Input | Required | Default | Description |
|-------|----------|---------|-------------|
| `manifest` | No | `./Cargo.toml` | Path to Cargo.toml |
| `musl` | Yes | - | Use musl for Linux builds (static linking) |
| `extra_args` | No | `[]` | Additional args to append (JSON array) |
| `extra_env` | No | `{}` | Additional env vars to merge (JSON object) |

The `extra_args` and `extra_env` inputs are merged with values from `Cargo.toml`. This is useful for secrets that only live in CI:

```yaml
jobs:
  build:
    permissions:
      contents: write
    uses: symposium-dev/package-agent-extension/.github/workflows/build.yml@v1
    with:
      musl: true
      extra_env: '{"API_KEY": "${{ secrets.API_KEY }}"}'
```

### Cargo.toml Metadata

The workflow reads package metadata from your `Cargo.toml`. Configure runtime behavior via `[package.metadata.symposium]`:

```toml
[package]
name = "my-extension"
version = "0.1.0"
description = "An agent extension for X"

[package.metadata.symposium]
binary = "my-ext"              # Optional: defaults to package name
args = ["--acp", "--verbose"]  # Optional: arguments passed when spawning
env = { API_KEY = "default" }  # Optional: environment variables
```

All fields are optional. The generated `extension.json` includes these values so Symposium knows how to spawn the extension.

## Targets

The workflow builds for these platforms:

| Platform | Target | Runner |
|----------|--------|--------|
| macOS ARM | `aarch64-apple-darwin` | `macos-14` |
| macOS x64 | `x86_64-apple-darwin` | `macos-13` |
| Linux ARM | `aarch64-unknown-linux-{musl,gnu}` | `ubuntu-24.04-arm` |
| Linux x64 | `x86_64-unknown-linux-{musl,gnu}` | `ubuntu-latest` |
| Windows x64 | `x86_64-pc-windows-msvc` | `windows-latest` |

## Output

For each release, the workflow uploads:

- `{binary}-{os}-{arch}-v{version}.tar.gz` (Unix)
- `{binary}-{os}-{arch}-v{version}.zip` (Windows)
- `extension.json` - manifest for the ACP registry

## Requirements

Callers must set `permissions: contents: write` - the reusable workflow cannot elevate permissions.

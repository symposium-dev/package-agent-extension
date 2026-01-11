# Agent Guide: Setting Up a Symposium Extension

This guide walks you through setting up automated builds and releases for a Symposium agent extension.

## Prerequisites

- A Rust crate that builds an Agent Client Protocol (ACP) agent or agent extension
- A GitHub repository for the extension

## Step 1: Add Symposium Metadata to Cargo.toml

Add a `[package.metadata.symposium]` section to configure how your extension is spawned:

```toml
[package.metadata.symposium]
binary = "my-extension"           # Optional: defaults to package name
args = ["--mcp"]                  # Optional: arguments passed when spawning
env = { SOME_VAR = "value" }      # Optional: environment variables
```

All fields are optional. The `binary` field defaults to the package name.

## Step 2: Create the Release Workflow

Create `.github/workflows/release.yml`:

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
      musl: true
    secrets: inherit
```

Set `musl: true` for static Linux binaries (recommended), or `musl: false` for dynamically linked binaries.

### Passing Secrets

If your extension needs secrets at runtime, use `extra_env`:

```yaml
jobs:
  build:
    permissions:
      contents: write
    uses: symposium-dev/package-agent-extension/.github/workflows/build.yml@v1
    with:
      musl: true
      extra_env: '{"API_KEY": "${{ secrets.API_KEY }}"}'
    secrets: inherit
```

Values in `extra_env` are merged with values from `Cargo.toml`.

## Step 3: Set Up Automated Releases with release-plz

[release-plz](https://release-plz.ieni.dev/) automates version bumping and release creation based on conventional commits.

### Initialize release-plz

**Ask the user to run this command:**

```bash
release-plz init
```

This command:
- Creates `.github/workflows/release-plz.yml`
- Sets up necessary GitHub repository secrets
- Configures the release workflow

If they don't have release-plz installed, they can install it with:

```bash
cargo binstall release-plz
# or cargo install
```

### Configure Conventional Commits

release-plz uses [conventional commits](https://www.conventionalcommits.org/) to determine version bumps. Add this to the project's `AGENTS.md` file so agents follow the convention:

```markdown
## Commit Convention

This project uses conventional commits for automated releases:

- `fix: ...` → patch version bump (0.1.0 → 0.1.1)
- `feat: ...` → minor version bump (0.1.0 → 0.2.0)
- `feat!: ...` or `BREAKING CHANGE:` → major version bump (0.1.0 → 1.0.0)

Always use conventional commit prefixes.
```

## How It Works

1. Push commits to `main` using conventional commit messages
2. release-plz creates a PR with version bumps and changelog updates
3. Merge the PR to trigger a release
4. release-plz creates a GitHub release
5. The release triggers the build workflow
6. Build workflow compiles binaries for all platforms and uploads them with `extension.json`

## Build Targets

The workflow builds for these platforms:

| Platform | Target |
|----------|--------|
| macOS ARM | `aarch64-apple-darwin` |
| macOS x64 | `x86_64-apple-darwin` |
| Linux ARM | `aarch64-unknown-linux-musl` (or gnu) |
| Linux x64 | `x86_64-unknown-linux-musl` (or gnu) |
| Windows x64 | `x86_64-pc-windows-msvc` |

## Release Artifacts

Each release includes:

- `{binary}-macos-aarch64-v{version}.tar.gz`
- `{binary}-macos-x86_64-v{version}.tar.gz`
- `{binary}-linux-aarch64-v{version}.tar.gz`
- `{binary}-linux-x86_64-v{version}.tar.gz`
- `{binary}-windows-x86_64-v{version}.zip`
- `extension.json` - manifest for the ACP registry

## Checklist

- [ ] Added `[package.metadata.symposium]` to Cargo.toml (if needed)
- [ ] Created `.github/workflows/release.yml`
- [ ] User ran `release-plz init`
- [ ] Using conventional commits for changes

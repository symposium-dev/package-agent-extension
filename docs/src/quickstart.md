# Quick Start

Get automated builds and releases working in 5 minutes.

## 1. Add the Release Workflow

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

## 2. Configure Your Cargo.toml

Add symposium metadata to specify how your extension should be spawned:

```toml
[package.metadata.symposium]
args = ["--acp"]  # Arguments passed when spawning
```

See [Cargo Metadata](./cargo-metadata.md) for all options.

## 3. Create a Release

Create a GitHub release (manually or via [release-plz](./release-plz.md)) and the workflow will:

- Build binaries for all 5 platform targets
- Upload them to the release
- Generate and upload `extension.json`

## Next Steps

- [Set up automated releases with release-plz](./release-plz.md)
- [Learn about all Cargo.toml options](./cargo-metadata.md)
- [See all workflow inputs](./inputs.md)

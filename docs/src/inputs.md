# Workflow Inputs

The `package-agent-extension` workflow accepts these inputs:

## All Inputs

| Input | Required | Default | Description |
|-------|----------|---------|-------------|
| `manifest` | No | `./Cargo.toml` | Path to Cargo.toml |
| `musl` | Yes | - | Use musl for Linux builds (static linking) |
| `extra_args` | No | `[]` | Additional args to append (JSON array) |
| `extra_env` | No | `{}` | Additional env vars to merge (JSON object) |

## `manifest`

Path to the Cargo.toml file. Use this for workspaces or non-standard layouts:

```yaml
with:
  manifest: ./crates/my-extension/Cargo.toml
  musl: true
```

## `musl`

Required. Controls whether Linux builds use musl (static linking) or glibc (dynamic linking).

**Recommended: `musl: true`** for maximum portability—binaries work on any Linux system.

Use `musl: false` only if you need glibc-specific features.

## `extra_args`

Additional arguments appended to the `args` array from Cargo.toml. Passed as a JSON array string:

```yaml
with:
  musl: true
  extra_args: '["--feature", "extra"]'
```

If Cargo.toml has `args = ["--acp"]` and you pass `extra_args: '["--verbose"]'`, the final args are `["--acp", "--verbose"]`.

## `extra_env`

Additional environment variables merged with the `env` object from Cargo.toml. Passed as a JSON object string:

```yaml
with:
  musl: true
  extra_env: '{"API_KEY": "${{ secrets.API_KEY }}"}'
```

This is the recommended way to inject secrets—they stay in CI and never appear in Cargo.toml.

If both Cargo.toml and `extra_env` define the same variable, `extra_env` wins.

## Complete Example

```yaml
jobs:
  build:
    permissions:
      contents: write
    uses: symposium-dev/package-agent-extension/.github/workflows/build.yml@v1
    with:
      manifest: ./Cargo.toml
      musl: true
      extra_args: '["--verbose"]'
      extra_env: '{"API_KEY": "${{ secrets.API_KEY }}", "DEBUG": "true"}'
    secrets: inherit
```

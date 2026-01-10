# Cargo Metadata

Configure your extension's runtime behavior via `[package.metadata.symposium]` in Cargo.toml.

## All Options

```toml
[package]
name = "my-extension"
version = "0.1.0"
description = "An agent extension for X"

[package.metadata.symposium]
binary = "my-ext"              # Binary name (defaults to package name)
args = ["--mcp", "--verbose"]  # Arguments passed when spawning
env = { API_KEY = "default" }  # Environment variables
```

## Fields

### `binary`

The name of the binary to build and run. Defaults to the package name.

Use this if your binary name differs from your package name:

```toml
[package]
name = "symposium-ferris"

[package.metadata.symposium]
binary = "ferris"  # Build and run "ferris" instead of "symposium-ferris"
```

### `args`

Arguments passed to the binary when spawned by Symposium:

```toml
[package.metadata.symposium]
args = ["--mcp", "--verbose"]
```

### `env`

Environment variables set when spawning:

```toml
[package.metadata.symposium]
env = { LOG_LEVEL = "info", FEATURE_FLAG = "enabled" }
```

For secrets, use `extra_env` in the workflow instead—see [Workflow Inputs](./inputs.md).

## No Metadata Required

All fields are optional. If you don't specify `[package.metadata.symposium]`, the workflow uses sensible defaults:

- `binary` → package name
- `args` → empty array
- `env` → empty object

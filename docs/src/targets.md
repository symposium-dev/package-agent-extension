# Build Targets

The workflow builds for 5 platform targets using native GitHub Actions runners.

## Platforms

| Platform | Target | Runner | Static |
|----------|--------|--------|--------|
| macOS ARM | `aarch64-apple-darwin` | `macos-14` | No |
| macOS x64 | `x86_64-apple-darwin` | `macos-13` | No |
| Linux ARM | `aarch64-unknown-linux-musl` | `ubuntu-24.04-arm` | Yes |
| Linux x64 | `x86_64-unknown-linux-musl` | `ubuntu-latest` | Yes |
| Windows x64 | `x86_64-pc-windows-msvc` | `windows-latest` | No |

When `musl: false`, Linux targets use `-gnu` instead of `-musl`.

## Native ARM Builds

Both macOS and Linux ARM builds use native ARM runners:

- **macOS ARM**: `macos-14` (Apple Silicon)
- **Linux ARM**: `ubuntu-24.04-arm` (AWS Graviton)

This means no cross-compilation or emulation—builds are fast and reliable.

## Static vs Dynamic Linking

**Linux with musl** produces fully static binaries that work on any Linux system without runtime dependencies.

**macOS and Windows** use dynamic linking to system libraries, which is standard for those platforms.

## Release Artifacts

Each release includes:

| File | Platform |
|------|----------|
| `{binary}-macos-aarch64-v{version}.tar.gz` | macOS ARM |
| `{binary}-macos-x86_64-v{version}.tar.gz` | macOS x64 |
| `{binary}-linux-aarch64-v{version}.tar.gz` | Linux ARM |
| `{binary}-linux-x86_64-v{version}.tar.gz` | Linux x64 |
| `{binary}-windows-x86_64-v{version}.zip` | Windows x64 |
| `extension.json` | Manifest for ACP registry |

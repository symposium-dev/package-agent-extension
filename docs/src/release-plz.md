# Automated Releases with release-plz

[release-plz](https://release-plz.ieni.dev/) automates version bumping and release creation based on conventional commits. This is the recommended way to manage releases.

## How It Works

1. Push commits to `main` using conventional commit messages
2. release-plz creates a PR with version bumps and changelog updates
3. Merge the PR to trigger a release
4. release-plz creates a GitHub release
5. The release triggers the build workflow, which compiles and uploads binaries

## Setup

### Install release-plz

```bash
cargo install release-plz
```

### Initialize

Run this in your repository:

```bash
release-plz init
```

This command:
- Creates `.github/workflows/release-plz.yml`
- Sets up necessary GitHub repository secrets
- Configures the release workflow

## Conventional Commits

release-plz uses [conventional commits](https://www.conventionalcommits.org/) to determine version bumps:

| Commit prefix | Version bump | Example |
|---------------|--------------|---------|
| `fix:` | Patch (0.1.0 → 0.1.1) | `fix: handle empty input` |
| `feat:` | Minor (0.1.0 → 0.2.0) | `feat: add new command` |
| `feat!:` or `BREAKING CHANGE:` | Major (0.1.0 → 1.0.0) | `feat!: change API` |

Other prefixes like `docs:`, `chore:`, `refactor:` don't trigger version bumps but are included in the changelog.

## Complete Example

With both workflows set up, your release process is:

1. Make changes with conventional commits:
   ```
   git commit -m "feat: add caching support"
   git push origin main
   ```

2. release-plz opens a PR bumping version to 0.2.0

3. Merge the PR

4. release-plz creates a GitHub release "v0.2.0"

5. The release triggers the build workflow

6. Binaries and `extension.json` are uploaded to the release

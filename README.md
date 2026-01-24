<div align="center">
  <img src="assets/simple-release2.0.png" alt="Simple Release Logo" width="200">
  <h1>Simple Release</h1>
</div>

<div align="center">

[![License: MIT](https://img.shields.io/badge/License-MIT-yellow.svg)](https://opensource.org/licenses/MIT)
[![GitHub release](https://img.shields.io/github/release/0xjuanma/simple-release.svg)](https://github.com/0xjuanma/simple-release/releases)

Speed up your release process: create a tag to trigger a new GitHub release, automatically update your changelog with release details, and receive a pull request with the latest changelog changes.
</div>

## Quick Setup

### 1. Add Release Workflow

Create `.github/workflows/release.yml`:

```yaml
name: Release
on:
  push:
    tags: ['v*.*.*']
permissions:
  contents: write
jobs:
  release:
    # Use @main for newest version, or pin to specific version like @v1.0.0
    uses: 0xjuanma/simple-release/.github/workflows/reusable-release.yml@main
    with:
      changelog-path: 'CHANGELOG.md'  # Optional: Changelog path
    secrets:
      RELEASE_TOKEN: ${{ secrets.GITHUB_TOKEN }}  # Optional: For creating releases
```

### 2. Add Post-Release Workflow

Create `.github/workflows/post-release-changelog.yml`:

```yaml
name: Post-Release Changelog Update
on:
  repository_dispatch:
    types: [release-published]
permissions:
  contents: write
  pull-requests: write
jobs:
  update-changelog:
    # Use @main for newest version, or pin to specific version like @v1.0.0
    uses: 0xjuanma/simple-release/.github/workflows/reusable-post-release-changelog.yml@main
    secrets:
      TOKEN: ${{ secrets.GITHUB_TOKEN }}  # Required: Creates PR and updates changelog
```

> **Note**: Each workflow uses different secret names:
> - **Release workflow**: Uses `RELEASE_TOKEN` (optional) for creating releases
> - **Post-release workflow**: Uses `TOKEN` (required) for creating PRs and updating changelog
> 
> You can pass your `GITHUB_TOKEN` to both, or create a Personal Access Token (PAT) with `repo` permissions and add it as a repository secret.

### 3. Add Tap Update Workflow (Optional)

For projects with Homebrew taps, create `.github/workflows/update-tap.yml`:

```yaml
name: Update Homebrew Tap
on:
  repository_dispatch:
    types: [release-published]
permissions:
  contents: read
jobs:
  update-tap:
    # Use @main for newest version, or pin to specific version like @v1.0.0
    uses: 0xjuanma/simple-release/.github/workflows/reusable-update-tap.yml@main
    with:
      tap-repo: 'yourusername/homebrew-tap'
      project-name: 'your-project'
      formula-path: 'your-project.rb'
      github-repo: 'yourusername/your-project'
      version: ${{ github.event.client_payload.version }}  # Required when triggered via repository_dispatch
    secrets:
      token: ${{ secrets.TAP_TOKEN }}
```

> **Note**: The `version` input is required when the workflow is triggered via `repository_dispatch` because `GITHUB_REF_NAME` will be `main` instead of the tag. The version is passed from the release workflow's `client-payload`.

### 4. Enable GitHub Actions to Create PRs

Enable PRs creation, see [Troubleshooting Token Issues](#troubleshooting-token-issues) for detailed steps.

### 5. Create Changelog

Create `CHANGELOG.md`:

```markdown
# Changelog

## [Unreleased]

### Added
- Your new features here

## [1.0.0] - 2024-01-01

### Added
- Initial release
```

> **Note**: As long as you keep your `CHANGELOG.md` up to date after each change, this workflow will automatically move all items under "Unreleased" into a new release section whenever you push a new tag.

## Usage

1. **Push a tag**: `git tag v1.0.0 && git push origin v1.0.0`
2. **Release created** with changelog content (no files attached)
3. **Formula updated** → Users can `brew upgrade` immediately (optional)
4. **Changelog updated** automatically
5. **PR created** with changelog changes

## Version Management

### Using @main (Recommended)
- **Always gets the newest version** with latest fixes and features
- **Automatic updates** when new changes are pushed to main
- **Best for most users** who want the latest functionality

```yaml
uses: 0xjuanma/simple-release/.github/workflows/reusable-release.yml@main
```

### Pinning to Specific Versions
- **Use for production** where you need stability
- **Prevents unexpected changes** from automatic updates
- **Update manually** when you want new features

```yaml
uses: 0xjuanma/simple-release/workflows/reusable-release.yml@v1.0.0
```

### Available Versions
Check [releases page](https://github.com/0xjuanma/simple-release/releases) for all available versions.

## Required Permissions

### GitHub Token Requirements
- **`GITHUB_TOKEN`**: Automatically provided by GitHub Actions
- **Permissions needed**: 
  - `contents: write` (to update changelog)
  - `pull-requests: write` (to create PR)
  - `metadata: read` (to read repository info)
- **Fallback option**: If default token fails, create a Personal Access Token (PAT) with `repo` scope and add as repository secret

### Troubleshooting Token Issues
If the post-release workflow fails to create PRs:

#### Fix: Enable GitHub Actions to Create PRs
1. Go to your repository on GitHub
2. Click **Settings → Actions → General**
3. Scroll down to **"Workflow permissions"**
4. Select **"Read and write permissions"**
5. ✅ Check **"Allow GitHub Actions to create and approve pull requests"**
6. Click **Save**

#### Alternative: Use Personal Access Token
If the above doesn't work:
1. **Create a PAT**: Go to GitHub Settings → Developer settings → Personal access tokens
2. **Required scope**: Select `repo` (full repository access)
3. **Add as secret**: Repository Settings → Secrets → Add `GITHUB_TOKEN` with your PAT
4. **Test**: Push a new tag to verify the workflow works

> **Note**: For production use, consider pinning to specific version tags (e.g., `@v1.0.0`) instead of `@main` to ensure you're using a stable, tested version.


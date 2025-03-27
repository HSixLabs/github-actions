# Semantic Release Action

This action provides a standardized way to manage versioning and releases using semantic versioning principles.

## Overview

This action handles:

1. Determining the next version based on conventional commits
2. Generating release notes from commit messages
3. Creating GitHub releases with appropriate tags
4. Publishing artifacts to registries when applicable
5. Ensuring consistent release workflows across projects

## Usage

```yaml
- name: Semantic Release
  id: semantic-release
  uses: hsixlabs/github-actions/semantic-release@main
  with:
    github_token: ${{ secrets.GITHUB_TOKEN }}
```

## Inputs

| Name | Description | Required | Default |
|------|-------------|----------|---------|
| `github_token` | GitHub token for creating releases | Yes | |
| `node_version` | Node.js version to use | No | `18` |
| `branches` | Branches on which releases should happen | No | `['main']` |
| `registry_url` | URL of the package registry | No | `https://registry.npmjs.org/` |
| `npm_token` | NPM token for publishing packages | No | |
| `docker_username` | Docker username for publishing images | No | |
| `docker_password` | Docker password for publishing images | No | |
| `docker_registry` | Docker registry URL | No | `docker.io` |
| `plugins` | Additional semantic-release plugins to install | No | |
| `dry_run` | Run in dry-run mode without making changes | No | `false` |
| `config_path` | Path to custom release configuration | No | |
| `release_directory` | Directory containing releasable artifacts | No | `.` |

## Outputs

| Name | Description |
|------|-------------|
| `new_release_published` | Whether a new release was published (`true` or `false`) |
| `new_release_version` | The version of the new release |
| `new_release_major_version` | The major version of the new release |
| `new_release_minor_version` | The minor version of the new release |
| `new_release_patch_version` | The patch version of the new release |
| `new_release_notes` | The release notes for the new release |

## Examples

### Basic Usage

```yaml
- name: Semantic Release
  id: semantic-release
  uses: hsixlabs/github-actions/semantic-release@main
  with:
    github_token: ${{ secrets.GITHUB_TOKEN }}
```

### Publishing an NPM Package

```yaml
- name: Semantic Release
  id: semantic-release
  uses: hsixlabs/github-actions/semantic-release@main
  with:
    github_token: ${{ secrets.GITHUB_TOKEN }}
    npm_token: ${{ secrets.NPM_TOKEN }}
```

### Publishing a Docker Image

```yaml
- name: Semantic Release
  id: semantic-release
  uses: hsixlabs/github-actions/semantic-release@main
  with:
    github_token: ${{ secrets.GITHUB_TOKEN }}
    docker_username: ${{ secrets.DOCKER_USERNAME }}
    docker_password: ${{ secrets.DOCKER_PASSWORD }}
    docker_registry: ghcr.io
```

### Using Custom Configuration

```yaml
- name: Semantic Release
  id: semantic-release
  uses: hsixlabs/github-actions/semantic-release@main
  with:
    github_token: ${{ secrets.GITHUB_TOKEN }}
    config_path: ./release.config.js
```

### Using Custom Plugins

```yaml
- name: Semantic Release
  id: semantic-release
  uses: hsixlabs/github-actions/semantic-release@main
  with:
    github_token: ${{ secrets.GITHUB_TOKEN }}
    plugins: |
      @semantic-release/commit-analyzer
      @semantic-release/release-notes-generator
      @semantic-release/github
      @semantic-release/git
      @semantic-release/npm
```

## How It Works

1. Sets up Node.js environment
2. Installs semantic-release and required plugins
3. Runs semantic-release with the provided configuration
4. Determines the next version based on conventional commits since the last release
5. Creates a GitHub release with release notes
6. Publishes artifacts to registries when credentials are provided
7. Outputs release information for use in subsequent workflow steps

## Conventional Commits

This action relies on [Conventional Commits](https://www.conventionalcommits.org/) for determining the next version. The commit types that trigger version increments are:

- `feat:` - Minor version increment (e.g., 1.0.0 → 1.1.0)
- `fix:` - Patch version increment (e.g., 1.0.0 → 1.0.1)
- `perf:` - Patch version increment (e.g., 1.0.0 → 1.0.1)
- `BREAKING CHANGE:` in footer or `!` after type - Major version increment (e.g., 1.0.0 → 2.0.0)

## Troubleshooting

### Common Issues

- **No new release created**: Ensure your commits follow the conventional commits format
- **Release created but artifacts not published**: Check registry credentials and permissions
- **GitHub rate limiting**: Use a personal access token with appropriate scopes instead of `GITHUB_TOKEN`

### Debugging

To debug release issues:

1. Set `dry_run: true` to see what would happen without making changes
2. Check the workflow logs for detailed information about the release process
3. Review your commit messages to ensure they follow the conventional commits format

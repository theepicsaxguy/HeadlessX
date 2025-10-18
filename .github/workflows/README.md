# GitHub Workflows

## Docker Build and Push Workflow

This workflow automatically builds and pushes the HeadlessX Docker image to GitHub Container Registry (GHCR).

### Triggers

1. **On Release**: Automatically triggered when a new release is published
2. **Manual Dispatch**: Can be manually triggered with a specific upstream version

### Features

- **Unique Tagging**: Each image is tagged with:
  - Upstream HeadlessX version (e.g., `v1.2.0`)
  - Latest tag (`latest`)
  - Git commit SHA (e.g., `sha-abc1234`)
  - Branch name (when applicable)
  
- **Upstream Version Sync**: Fetches the latest version tag from the upstream repository (https://github.com/saifyxpro/HeadlessX/releases)

- **Multi-platform Support**: Builds for `linux/amd64` platform

- **Build Cache**: Uses GitHub Actions cache for faster builds

### Manual Trigger

To manually trigger the workflow:

1. Go to Actions tab in GitHub
2. Select "Build and Push Docker Image" workflow
3. Click "Run workflow"
4. Enter the upstream version (e.g., `v1.2.0`)
5. Click "Run workflow"

### Image Location

After the workflow runs, images will be available at:

```
ghcr.io/theepicsaxguy/headlessx:latest
ghcr.io/theepicsaxguy/headlessx:v1.2.0
ghcr.io/theepicsaxguy/headlessx:sha-abc1234
```

### Using the Image

```bash
# Pull the latest image
docker pull ghcr.io/theepicsaxguy/headlessx:latest

# Pull a specific version
docker pull ghcr.io/theepicsaxguy/headlessx:v1.2.0

# Run the container
docker run -p 3000:3000 -e AUTH_TOKEN=your_token ghcr.io/theepicsaxguy/headlessx:latest
```

### Permissions

The workflow requires:
- `contents: read` - To checkout the repository
- `packages: write` - To push to GitHub Container Registry

# Docker Build and Push Workflow

## Overview

This repository now includes a GitHub Actions workflow that automatically builds and pushes Docker images to GitHub Container Registry (GHCR).

## Workflow Details

### File Location
`.github/workflows/docker-build-push.yml`

### Triggers

The workflow can be triggered in two ways:

#### 1. Automatic Trigger (On Release)
When a new release is published in this repository, the workflow automatically:
- Fetches the latest release version from the upstream repository (https://github.com/saifyxpro/HeadlessX)
- Builds the Docker image
- Tags it with the upstream version
- Pushes to GHCR

#### 2. Manual Trigger (Workflow Dispatch)
You can manually trigger the workflow from the GitHub Actions tab:
1. Navigate to **Actions** → **Build and Push Docker Image**
2. Click **Run workflow**
3. Enter the upstream version (e.g., `v1.2.0`)
4. Click **Run workflow**

## Image Tags

Each build creates multiple tags for flexibility:

| Tag Type | Example | Description |
|----------|---------|-------------|
| **Upstream Version** | `v1.2.0` | Matches the upstream HeadlessX release version |
| **Latest** | `latest` | Always points to the most recent build |
| **Commit SHA** | `sha-abc1234` | Unique identifier based on git commit |
| **Branch** | `main` | Branch-based tag (when applicable) |

## Image Location

All images are pushed to:
```
ghcr.io/theepicsaxguy/headlessx
```

### Pull Commands

```bash
# Pull the latest image
docker pull ghcr.io/theepicsaxguy/headlessx:latest

# Pull a specific upstream version
docker pull ghcr.io/theepicsaxguy/headlessx:v1.2.0

# Pull by commit SHA (useful for debugging)
docker pull ghcr.io/theepicsaxguy/headlessx:sha-abc1234
```

## Using the Image

### Basic Usage

```bash
docker run -p 3000:3000 \
  -e AUTH_TOKEN=your_secure_token \
  -e DOMAIN=yourdomain.com \
  -e SUBDOMAIN=headlessx \
  ghcr.io/theepicsaxguy/headlessx:latest
```

### With Environment File

```bash
# Create .env file
cat > .env << EOF
AUTH_TOKEN=your_secure_token
DOMAIN=yourdomain.com
SUBDOMAIN=headlessx
PORT=3000
NODE_ENV=production
EOF

# Run with environment file
docker run -p 3000:3000 --env-file .env ghcr.io/theepicsaxguy/headlessx:latest
```

### Using Docker Compose

If you prefer using docker-compose, you can modify the existing `docker/docker-compose.yml`:

```yaml
services:
  headlessx:
    image: ghcr.io/theepicsaxguy/headlessx:latest
    container_name: headlessx
    ports:
      - "3000:3000"
    env_file:
      - ../.env
    restart: unless-stopped
    volumes:
      - ../logs:/app/logs
```

Then run:
```bash
docker-compose up -d
```

## Workflow Configuration

### Key Features

1. **Multi-stage Build**: Uses the existing multi-stage Dockerfile for optimized image size
2. **Build Cache**: Leverages GitHub Actions cache for faster builds
3. **Platform Support**: Currently builds for `linux/amd64`
4. **Security**: Uses GitHub's built-in `GITHUB_TOKEN` for authentication

### Permissions

The workflow requires the following permissions:
- `contents: read` - To checkout the repository code
- `packages: write` - To push images to GitHub Container Registry

These permissions are automatically granted by GitHub for workflows in the repository.

## Upstream Version Synchronization

The workflow automatically fetches version information from the upstream repository:
- **Upstream Repository**: https://github.com/saifyxpro/HeadlessX
- **API Endpoint**: `https://api.github.com/repos/saifyxpro/HeadlessX/releases/latest`

This ensures that your Docker images are always tagged with the corresponding upstream HeadlessX version, making it easy to track which version of HeadlessX is included in each image.

## Version Format

The workflow validates that upstream versions follow the semantic versioning format:
```
v<major>.<minor>.<patch>
```

Examples:
- ✅ `v1.2.0`
- ✅ `v1.3.0`
- ❌ `1.2.0` (missing 'v' prefix)
- ❌ `v1.2` (missing patch version)

## Monitoring Builds

### View Workflow Runs

1. Go to the **Actions** tab in your GitHub repository
2. Select **Build and Push Docker Image** workflow
3. View individual workflow runs and their logs

### Check Build Status

The workflow run will show:
- Build progress
- Tag creation
- Push status to GHCR
- Image digest

### Troubleshooting

If a build fails:
1. Check the workflow logs in the Actions tab
2. Verify the Dockerfile syntax
3. Ensure all required files are present in the repository
4. Check that the upstream repository is accessible

## Best Practices

1. **Always use version tags in production**
   ```bash
   docker pull ghcr.io/theepicsaxguy/headlessx:v1.2.0
   ```

2. **Use latest tag for development/testing only**
   ```bash
   docker pull ghcr.io/theepicsaxguy/headlessx:latest
   ```

3. **Pin to specific commits for debugging**
   ```bash
   docker pull ghcr.io/theepicsaxguy/headlessx:sha-abc1234
   ```

## Image Visibility

By default, GitHub Container Registry images are public. To use them:
- No authentication required for pulling public images
- Authentication only required for pushing (handled automatically by the workflow)

To make images private:
1. Go to the package settings in GitHub
2. Change visibility to private
3. Configure authentication for pulling

## Related Documentation

- [Dockerfile](../docker/Dockerfile)
- [Docker Compose Configuration](../docker/docker-compose.yml)
- [Deployment Guide](../README.md#deployment-guide)
- [GitHub Actions Documentation](https://docs.github.com/en/actions)
- [GitHub Container Registry Documentation](https://docs.github.com/en/packages/working-with-a-github-packages-registry/working-with-the-container-registry)

## Support

If you encounter issues with the Docker workflow:
1. Check the [GitHub Actions logs](../../actions)
2. Review the [Troubleshooting Guide](TROUBLESHOOTING.md)
3. Open an issue in the repository

---

**Note**: This workflow is specifically designed to track the upstream HeadlessX releases from https://github.com/saifyxpro/HeadlessX and tag images accordingly.

# Docker Build and Push Setup - Quick Reference

## What Was Added

This repository now has automated Docker image building and publishing to GitHub Container Registry (GHCR).

## Files Created

1. **`.github/workflows/docker-build-push.yml`** - Main workflow file
2. **`.github/workflows/README.md`** - Quick workflow documentation
3. **`docs/DOCKER_WORKFLOW.md`** - Comprehensive documentation

## Quick Start

### Automatic Build (Recommended)
Simply create a release in this repository:
1. Go to **Releases** → **Create a new release**
2. Add a tag and release notes
3. Publish the release
4. The workflow automatically builds and pushes the Docker image

### Manual Build
From the GitHub Actions tab:
1. Go to **Actions** → **Build and Push Docker Image**
2. Click **Run workflow**
3. Enter upstream version (e.g., `v1.2.0`)
4. Click **Run workflow**

## Image Tags

Your images will be available at:
```
ghcr.io/theepicsaxguy/headlessx:latest
ghcr.io/theepicsaxguy/headlessx:v1.2.0
ghcr.io/theepicsaxguy/headlessx:sha-abc1234
```

## Usage Example

```bash
# Pull the image
docker pull ghcr.io/theepicsaxguy/headlessx:latest

# Run the container
docker run -d \
  -p 3000:3000 \
  -e AUTH_TOKEN=your_token \
  -e DOMAIN=yourdomain.com \
  -e SUBDOMAIN=headlessx \
  --name headlessx \
  ghcr.io/theepicsaxguy/headlessx:latest
```

## Key Features

✅ **Automatic upstream version tracking** - Syncs with https://github.com/saifyxpro/HeadlessX releases  
✅ **Unique image tags** - SHA, version, and latest tags for all builds  
✅ **Build caching** - Fast builds using GitHub Actions cache  
✅ **Secure** - Uses GitHub's built-in authentication  
✅ **Multi-trigger** - Automatic on release or manual dispatch  

## Verification

To verify the workflow is working:

1. **Check workflow syntax**: ✅ Validated
2. **Check Docker build**: ✅ Context verified
3. **Check YAML syntax**: ✅ Valid

## Next Steps

1. **Create a release** to trigger the first build
2. **Monitor the build** in the Actions tab
3. **Pull and test** the image from GHCR

## Support

- **Workflow Documentation**: [docs/DOCKER_WORKFLOW.md](docs/DOCKER_WORKFLOW.md)
- **GitHub Actions**: [Actions Tab](../../actions)
- **GHCR Packages**: [Packages](../../packages)

---

**Status**: ✅ Ready to use!

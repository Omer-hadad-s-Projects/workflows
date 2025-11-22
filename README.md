# Reusable Workflows

This repository contains a collection of reusable GitHub Actions workflows.

## Docker Image Builder

This workflow automates the process of tagging releases based on a changelog and building/pushing Docker images to Docker Hub.

### Workflow File

The reusable workflow is located at: `.github/workflows/docker image builder.yaml`

### Prerequisites & Rules

To successfully use this workflow, your repository must adhere to the following rules:

1.  **CHANGELOG.md**: 
    *   You must have a changelog file (default: `CHANGELOG.md`) in your repository.
    *   The workflow uses this file to determine the version for the release tag.
    *   Ensure your changelog follows a standard format where the latest version is identifiable.

2.  **Secrets**:
    *   You must configure the following secrets in your repository settings (Settings > Secrets and variables > Actions):
        *   `DOCKERHUB_USERNAME`: Your Docker Hub username.
        *   `DOCKERHUB_TOKEN`: A Docker Hub Access Token (preferred over password).

3.  **Dockerfile**:
    *   A valid `Dockerfile` must exist in the specified `docker_context` (default is the root `.`).

### Usage

Create a new workflow file in your repository (e.g., `.github/workflows/release.yml`) and use the following configuration:

```yaml
name: Release

on:
  push:
    branches:
      - master  # Trigger on pushes to your default branch

jobs:
  release:
    # ensure you reference the correct branch or tag (e.g., @master or @v1)
    uses: Omer-hadad-s-Projects/workflows/.github/workflows/docker-image-builder.yaml@master 
    with:
      # Required: The full image name (e.g., username/repo)
      image_name: your_dockerhub_username/your_image_name       
      
      # Optional: Default branch for the :latest tag (default: main)
      default_branch: master                    
      
      # Optional: Path to your changelog file (default: CHANGELOG.md)
      changelog_path: CHANGELOG.md             
      
      # Optional: Build context for Docker (default: .)
      docker_context: .                       
    secrets:
      DOCKERHUB_USERNAME: ${{ secrets.DOCKERHUB_USERNAME }}
      DOCKERHUB_TOKEN:   ${{ secrets.DOCKERHUB_TOKEN }}
```

### Inputs

| Input | Description | Required | Default |
|-------|-------------|----------|---------|
| `image_name` | The Docker image name (e.g., `myuser/my-app`). | **Yes** | N/A |
| `default_branch` | The default branch name. If the current branch matches this, the image will also be tagged `latest`. | No | `main` |
| `changelog_path` | Path to the CHANGELOG file relative to the root. | No | `CHANGELOG.md` |
| `docker_context` | The build context for the Docker image. | No | `.` |

### Secrets

| Secret | Description | Required |
|--------|-------------|----------|
| `DOCKERHUB_USERNAME` | Your Docker Hub username. | **Yes** |
| `DOCKERHUB_TOKEN` | Your Docker Hub access token. | **Yes** |

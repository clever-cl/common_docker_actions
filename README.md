# How to reuse this workflows

Example using [Docker release](.github/workflows/docker_release.yml) workflow:
```yaml
# Use this workflow to create and push both docker image and helm chart package.
name: Build & Release

on:
  # build and release a new helm package for specific tags.
  release:
    types: [prereleased, released]

jobs:
  build:
    # Github action path and version
    uses: biceventures/common_docker_actions/.github/workflows/docker_release.yml@stable
    # Require inputs of this workflow
    with:
      project_name: ${{ github.event.repository.name }}
      values_file_path: ./helm/chart/values.yaml
      chart_path: ./helm/chart/
    secrets:
      # For use GCP services with Workload Identity
      APP_KSA: ${{ secrets.APP_KSA }}
      APP_GSA: ${{ secrets.APP_GSA }}
      GOOGLE_PROJECT_ID: ${{ secrets.GOOGLE_PROJECT_ID }}
      # For use Docker Registry and Helm
      CLEVER_DOCKER_REGISTRY: ${{ secrets.CLEVER_DOCKER_REGISTRY }}
      CLEVER_HELM_REGISTRY: ${{ secrets.CLEVER_HELM_REGISTRY }}
```

Example using [Docker Security Scan](.github/workflows/docker_scan.yml) workflow:
```yaml
# Use this workflow to be sure that you have a secure docker image. 
name: Docker Security Scan

on:
  # build and test in PRs
  pull_request:
    paths:
     - Dockerfile
     - src/**

jobs:
  build:
    # Github action path and version
    uses: biceventures/common_docker_actions/.github/workflows/docker_scan.yml@stable
    # Require inputs of this workflow
    with:
      project_name: ${{ github.event.repository.name }}
    secrets:
      # For use Docker Registry
      CLEVER_DOCKER_REGISTRY: ${{ secrets.CLEVER_DOCKER_REGISTRY }}
```

Example using [Docker Security Scan with build args](.github/workflows/docker_scan.yml) workflow:
```yaml
# Use this workflow to be sure that you have a secure docker image. 
name: Docker Security Scan

on:
  # build and test in PRs
  pull_request:
    paths:
     - Dockerfile
     - src/**

jobs:
  build:
    # Github action path and version
    uses: biceventures/common_docker_actions/.github/workflows/docker_scan.yml@stable
    # Require inputs of this workflow
    with:
      project_name: ${{ github.event.repository.name }}
      build_args: NPM_TOKEN=${{ secrets.NPM_TOKEN }}
    secrets:
      # For use Docker Registry
      CLEVER_DOCKER_REGISTRY: ${{ secrets.CLEVER_DOCKER_REGISTRY }}
```
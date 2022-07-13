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
    uses: clever-cl/common_docker_actions/.github/workflows/docker_release.yml@stable
    # Require inputs of this workflow
    with:
      project_name: ${{ github.event.repository.name }}
      values_file_path: ./helm/chart/values.yaml
      chart_path: ./helm/chart/
      dockerfile_path: 'path/to/Dockerfile' # optional
      docker_context: 'path/to/build/context/' # optional
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
    uses: clever-cl/common_docker_actions/.github/workflows/docker_scan.yml@stable
    # Require inputs of this workflow
    with:
      project_name: ${{ github.event.repository.name }}
      dockerfile_path: 'path/to/Dockerfile' # optional
      docker_context: 'path/to/build/context/' # optional
    secrets:
      # For use Docker Registry
      CLEVER_DOCKER_REGISTRY: ${{ secrets.CLEVER_DOCKER_REGISTRY }}
```

Use the inherit keyword to implicitly pass the secrets.

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
    uses: clever-cl/common_docker_actions/.github/workflows/docker_scan.yml@stable
    # Require inputs of this workflow
    with:
      project_name: ${{ github.event.repository.name }}
      dockerfile_path: 'path/to/Dockerfile' # optional
      docker_context: 'path/to/build/context/' # optional
    secrets: inherit
```

Example using [Lint and Test Helm Charts](.github/workflows/helm_lint.yml) workflow:
```yaml
# Use this workflow to create and push both docker image and helm chart package.
name: Helm Lint and Test Charts

on: pull_request

jobs:
  build:
    # Github action path and version
    uses: clever-cl/common_docker_actions/.github/workflows/helm_lint.yml@stable
    # Require inputs of this workflow
    with:
      project_name: ${{ github.event.repository.name }}
      values_file_path: ./helm/chart/values.yaml
      chart_path: ./helm/chart/
    secrets: inherit
```
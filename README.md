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
    uses: biceventures/common_docker_actions/.github/workflows/docker_release.yml@master
    # Require inputs of this workflow
    with:
      project_name: my-app
      values_file_path: ./helm/chart/values.yaml
      chart_path: ./helm/chart/
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
    uses: biceventures/common_docker_actions/.github/workflows/docker_scan.yml@master
    # Require inputs of this workflow
    with:
      project_name: my-app
```
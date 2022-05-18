# How to reuse this workflows

```yaml
name: Build & Release

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
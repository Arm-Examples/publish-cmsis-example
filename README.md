# publish-cmsis-example

This repository contains the GitHub action used to publish CMSIS example to [keil.arm.com](https://keil.arm.com)

## Usage

In the steps in your workflow file:
```yaml
- uses: Arm-Examples/publish-cmsis-example@latest
  with:
    branch:            # Branch to get the project from if not `main`
    project-file:      # Path to the project to publish (file with `.csolution.yml` extension) (required)
    project-directory: # Path to the project directory if not git repository root
    dry-run:           # Whether to just complete a dry-run (`true` or `false`). This can be useful to verify a project is publishable without actually making it available on `keil.arm.com`
    CMSIS_API_KEY:     # API token for using CMSIS services (required)
```

All inputs marked with `(required)` are required.

### Example: Single CI Workfow

A single CI workflow can be used if a user only wants to publish when the CI runs against a specific branch but still wants the validation to be run when the CI runs against other branches (e.g. in pull requests). To do this you can set the `dry-run` to evaluate to true only if you are running against the specific branch:

```yaml
# ci.yml
name: CI

on:
  push:
    branches: [ main ]
  pull_request:
    branches: [ main ]

jobs:
  publish-project-action:
    name: Publish Action
    runs-on: ubuntu-latest
    steps:
      - uses: actions/checkout@v4
      - uses: Arm-Examples/publish-cmsis-example@latest
        name: Publish Project Action
        with:
          branch: ${{ github.ref }}
          project-file: ./hello.csolution.yml
          dry-run: ${{ github.ref != 'refs/heads/main' }} # This will evaluate to true if the branch is not main
          CMSIS_API_KEY: ${{ secrets.CMSIS_API_KEY }}
```

### Example: Multiple Workflows

As an alternative to the previous approach, you can have two CI workflows. One will run against pull requests and another will run against pushes to the `main` branch:

Complete a dry-run to validate that the project is publishable on pull requests:

```yaml
# ci-pull-requests.yml
name: CI Pull Requests

on:
  pull_request:
    branches: [ main ]

jobs:
  validate-action:
    name: Validate action is publishable
    runs-on: ubuntu-latest
    steps:
      - uses: actions/checkout@v4
      - uses: Arm-Examples/publish-cmsis-example@latest
        name: Publish Project Action
        with:
          branch: ${{ github.ref }}
          project-file: ./hello.csolution.yml
          dry-run: true
          CMSIS_API_KEY: ${{ secrets.CMSIS_API_KEY }}
```

On the `main` branch, publish the action as `dry-run` is set to `false`:

```yaml
# ci-main.yml
name: CI Main

on:
  push:
    branches: [ main ]

jobs:
  publish-project-action:
    name: Publish Action
    runs-on: ubuntu-latest
    steps:
      - uses: actions/checkout@v4
      - uses: Arm-Examples/publish-cmsis-example@latest
        name: Publish Project Action
        with:
          branch: main
          project-file: ./hello.csolution.yml
          dry-run: false
          CMSIS_API_KEY: ${{ secrets.CMSIS_API_KEY }}
```

## API Token Access

Use of this GitHub Action requires an API token. To request a token for use in your own organization please contact any of the repository contributors.

---

**_Note: This action publishes example projects to [keil.arm.com](https://keil.arm.com). Please note that this process can take up to two hours to complete so projects may not be immediately visible._**

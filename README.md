# publish-cmsis-example

This repository contains the GitHub action used to publish CMSIS example to [keil.arm.com](https://keil.arm.com)

## Usage

In the steps in your workflow file:
```yaml
- uses: Arm-Examples/publish-cmsis-example@latest
  with:
    branch:       # Branch to get the project from if not `main`
    project-file: # Path to the project to publish (file with `.csolution.yml` extension) (required)
    dry-run:      # Whether to just complete a dry-run (`true` or `false`). This can be useful to verify a project is publishable without actually making it available on `keil.arm.com`
    API_TOKEN:    # API token for publishing projects (required)
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
          API_TOKEN: ${{ secrets.KEIL_API_TOKEN }}
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
          API_TOKEN: ${{ secrets.KEIL_API_TOKEN }}
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
          API_TOKEN: ${{ secrets.KEIL_API_TOKEN }}
```

## Tokens

@willlordarm You wanted a comment here about the tokens.

---

**_Note: This action will update the datastore for [keil.arm.com](https://keil.arm.com), and the changes will later be picked up and displayed on the website. Hence, a project published using this action will not appear on the website immediately but within a couple of hours._**

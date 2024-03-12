# publish-cmsis-example

This repository contains the GitHub action used to publish CMSIS example to [keil.arm.com](https://keil.arm.com)

## Usage

In the steps in your workflow file:
```yaml
- uses: Arm-Debug/solar-pack-project-importer@latest
  with:
    branch: # Branch to run on
    project-file: # Project to import (required)
    dry-run: # Whether to just complete a dry-run. This can be useful to verify a project is convertable and uploadable without actually making it available on keil.arm.com
    SOFTWARE_API_TOKEN: # API token with push access to the software-api
```

All inputs marked with `(required)` are required.

# publish-cmsis-example

This repository contains the GitHub action used to publish CMSIS example to [keil.arm.com](https://keil.arm.com)

## Usage

In the steps in your workflow file:
```yaml
- uses: Arm-Examples/publish-cmsis-example@latest
  with:
    branch:       # Branch to run on
    project-file: # Project to import (required)
    dry-run:      # Whether to just complete a dry-run. This can be useful to verify a project is publishable without actually making it available on keil.arm.com
    API_TOKEN:    # API token for publishing projects
```

All inputs marked with `(required)` are required.

---

**_Note: This action will update the cache for [keil.arm.com](https://keil.arm.com), the changes are applied via a cronjob so a project published using this action will not appear on the website immediately._**

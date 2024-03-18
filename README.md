# publish-cmsis-example

This repository contains the GitHub action used to publish CMSIS example to [keil.arm.com](https://keil.arm.com)

## Usage

In the steps in your workflow file:
```yaml
- uses: Arm-Examples/publish-cmsis-example@latest
  with:
    branch:       # Branch to get the project from if not `main`
    project-file: # path to the project to publish (file with `.csolution.yml` extension) (required)
    dry-run:      # Whether to just complete a dry-run. This can be useful to verify a project is publishable without actually making it available on `keil.arm.com`
    API_TOKEN:    # API token for publishing projects (required)
```

All inputs marked with `(required)` are required.

---

**_Note: This action will update the datastore for [keil.arm.com](https://keil.arm.com), and the changes will later be picked up and displayed on the website. Hence, a project published using this action will not appear on the website immediately but within a couple of hours._**

<!--
Copyright (C) 2020-2024 Arm Limited or its affiliates and Contributors. All rights reserved.
SPDX-License-Identifier: Apache-2.0
-->

# Dependency upgrades

For dependency upgrades, dependabot is relied upon and news files are auto-generated in order to document such change. 

# Releasing

## Release Types

The CI supports three release flows:

- `development` for snapshot releases
- `release` for stable releases
- `beta` for pre-releases


|   Type      |   Purpose   | Version Number Format | GitHub Release | News Files Deleted |
|-------------|-------------|-----------------------|:--------------:|:------------------:|
| Release     | General Availability | `<minor>.<major>.<patch>`                            | Yes | Yes |
| Beta        | Integration Testing  | `<minor>.<major>.<patch>-beta.<commit number>`       | Yes | No  |
| Development | Development Testing  | `<minor>.<major>.<patch>-dev+<git hash>`             | No  | No  |

> :warning: releases can be made from any branches but
> it is recommended that they are only made from the `main` branch.

### Release workflow

1. Navigate to the [GitHub Actions](https://github.com/Arm-Examples/publish-cmsis-example/actions/workflows/release.yml) page.
2. Select the **Run Workflow** button and type which kind of release you would like to make (i.e. release, beta or development).

### Version Numbers

The version number will be automatically calculated, based on the news files.

# Detecting secrets

So that no secrets are committed back to the repository, a combination of two tools are run in CI:
- [GitLeaks]() : Scans the git history for usual secrets (e.g. AWS keys, etc.)
- [detect-secrets](https://github.com/Yelp/detect-secrets): Scans only the current state of the repository for anything which can look like secrets (strings with high entropy)

For the latter, False positive keys are stored in the [baseline](./.secrets.baseline) which `detect-secrets` checks against when it runs

## Baseline & False positives

To flag individual false positives add comment `# pragma: allowlist secret` to line with secret

To add all suspected secrets in the repository (excluding ones with an allow secret comment), run `detect-secrets scan --all-files --exclude-files '.*go\.sum$' --exclude-files '.*\.html$' --exclude-files '.*\.properties$' --exclude-files 'ci.yml' --exclude-files '\.git' > .secrets.baseline`

If on Windows: then change the encoding of the .secrets.baseline file to UTF-8 then convert all `\` to `/` in the .secrets.baseline file

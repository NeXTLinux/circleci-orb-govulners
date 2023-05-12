[![CircleCI Build Status](https://circleci.com/gh/nextlinux/circleci-orb-govulners.svg?style=shield "CircleCI Build Status")](https://circleci.com/gh/nextlinux/circleci-orb-govulners) 
[![CircleCI Orb Version](https://badges.circleci.com/orbs/nextlinux/govulners.svg)](https://circleci.com/orbs/registry/orb/nextlinux/govulners) 
[![CircleCI Community](https://img.shields.io/badge/community-CircleCI%20Discuss-343434.svg)](https://discuss.circleci.com/c/ecosystem/orbs)
[![License: Apache-2.0](https://img.shields.io/badge/License-Apache%202.0-blue.svg)](https://github.com/nextlinux/circleci-orb-govulners/blob/main/LICENSE)

# Govulners Orb for CircleCI

This repo is the source of the Govulners CircleCI Orb, which uses [Govulners](https://github.com/nextlinux/govulners) to scan vulnerabilities in container images. This orb has one command, it scans a given image from a registry, like docker.io. 

To use this orb in your repo add the following to the list of jobs in your `.circleci/config.yml` (assuming you already configured circleCI in your repo, if not please check [Getting Started with CircleCI](https://circleci.com/docs/2.0/getting-started/)):

``` yaml
orbs:
  govulners: nextlinux/govulners@<version>


workflows:
  integration-test_deploy:
    jobs:
      - govulners/list-image-vulns:
          image-name: ubuntu:20.04 # govulners will pull this image from docker.io, by default, check the scan-image command for more options 
      - govulners/list-dir-vulns:
          path-to-scan: ./
```

Check [Scan Image](src/commands/scan_image.yml) command for more options, such as: fail testing if an image has a vulnerability as severe or equal to `high`.

## Jobs
### govulners/list-image-vulns
List vulnerabilities for a given container image.

| Parameter           | Description                                                                                                                      | Default              |
|---------------------|----------------------------------------------------------------------------------------------------------------------------------|----------------------|
| image-name          | A container image to scan. (e.g. alpine:latest)                                                                                  |                      |
| output-format       | Report output formatter. Supported formats are: json, table, cyclonedx, template                                                 | table                |
| output-file         | File name where the list of vulnerabilities are saved.                                                                           | ./govulners-vulns.output |
| fail-on-severity    | Fail scanning if a vulnerability is found with a severity >= the given severity. One of: negligible, low, medium, high, critical |                      |
| enable-verbose-logs | Flag to enable verbose logs for govulners.                                                                                       | true                 |
| govulners-version       | Version of govulners used for orb                                                                                                    | v0.26.1              |

### govulners/list-dir-vulns
List vulnerabilities for a local directory path.

| Parameter           | Description                                                                                                                      | Default              |
|---------------------|----------------------------------------------------------------------------------------------------------------------------------|----------------------|
| path-to-scan        | Path to scan (e.g. "/home/user/project/abc" for absolute path, or "./abc" if the execution starts from "/home/user/project/")    |                      |
| output-format       | Report output formatter. Supported formats are: json, table, cyclonedx, template                                                 | table                |
| output-file         | File name where the list of vulnerabilities are saved.                                                                           | ./govulners-vulns.output |
| fail-on-severity    | Fail scanning if a vulnerability is found with a severity >= the given severity. One of: negligible, low, medium, high, critical |                      |
| enable-verbose-logs | Flag to enable verbose logs for govulners.                                                                                           | true                 |
| govulners-version       | Version of govulners used for orb                                                                                                    | v0.26.1              |

## Commands
### govulners/scan-image
Scan a Docker image with govulners.

| Parameter           | Description                                                                                                                      | Default              |
|---------------------|----------------------------------------------------------------------------------------------------------------------------------|----------------------|
| image-name          | A container image to scan. (e.g. alpine:latest)                                                                                  |                      |
| output-format       | Report output formatter. Supported formats are: json, table, cyclonedx, template                                                 | table                |
| output-file         | File name where the list of vulnerabilities are saved.                                                                           | ./govulners-vulns.output |
| fail-on-severity    | Fail scanning if a vulnerability is found with a severity >= the given severity. One of: negligible, low, medium, high, critical |                      |
| enable-verbose-logs | Flag to enable verbose logs for govulners.                                                                                           | true                 |
| govulners-version       | Version of govulners used for orb                                                                                                    | v0.26.1              |
| registry-address    | Name of private registry (e.g. docker.io, localhost:5000)                                                                        |                      |
| registry-user       | Username for private registry                                                                                                    |                      |
| registry-pass       | Password for private registry                                                                                                    |                      |
| registry-auth-token | Auth token for private registry                                                                                                  |                      |

### govulners/scan-path
Scan a given path with govulners.

| Parameter           | Description                                                                                                                      | Default              |
|---------------------|----------------------------------------------------------------------------------------------------------------------------------|----------------------|
| path-to-scan        | Path to scan (e.g. "/home/user/project/abc" for absolute path, or "./abc" if the execution starts from "/home/user/project/")    |                      |
| output-format       | Report output formatter. Supported formats are: json, table, cyclonedx, template                                                 | table                |
| output-file         | File name where the list of vulnerabilities are saved.                                                                           | ./govulners-vulns.output |
| fail-on-severity    | Fail scanning if a vulnerability is found with a severity >= the given severity. One of: negligible, low, medium, high, critical |                      |
| enable-verbose-logs | Flag to enable verbose logs for govulners.                                                                                           | true                 |
| govulners-version       | Version of govulners used for orb                                                                                                    | v0.26.1              |

## Development
All orbs are tested with .circleci/config.yaml of this repo. Finished orbs will be published to the public CircleCi orb repository under the nextlinux namespace.

* Orb testing will be initiated upon pushing to repo
* If orb passes linting & packing it will be published using `@dev:alpha`

After the `@dev:alpha` orb is successfully published, integration tests will be triggered. Once all tests have passed, the dev orb can be promoted to production. To View the current version of the orb, use the following command:

```
circleci orb info nextlinux/govulners
```

Use `Makefile` for repetitive operations such as: building, validation and publishing to CircleCI. 

### Publishing

1. **Open a new Pull Request to the default branch.** 
New releases are only published on merges to the default branch. The included .circleci/config.yml configuration file automatically packs, tests, and publishes your orbs. By default, both integration tests and unit tests are enabled for this CI pipeline. It is highly recommended that you add integration tests at a minimum to ensure the functionality of your orb.

2. **Ensure all tests pass.** 
You can view the results of your tests directly on GitHub within the Pull Request, or, for a more detailed view, watch the entire pipeline on CircleCI.com. 

3. **Title Pull Request with Special Semver Tag.**
The included CI config uses the orb-tools orb to automatically publish orbs that pass testing on the default branch, provided that the commit message contains the correct tag designated the intended semver release.
The tag template looks like this: [semver:<increment>], where <increment> is replaced with one of the following values:

| Increment | Description                       |
|-----------|-----------------------------------|
| major     | Issue a 1.0.0 incremented release |
| minor     | Issue a x.1.0 incremented release |
| patch     | Issue a x.x.1 incremented release |
| skip      | Do not issue a release            |

## Resources

[CircleCI Orb Registry Page](https://circleci.com/orbs/registry/orb/nextlinux/circleci-orb-govulners) - The official registry page of this orb for all versions, executors, commands, and jobs described.
[CircleCI Orb Docs](https://circleci.com/docs/2.0/orb-intro/#section=configuration) - Docs for using and creating CircleCI Orbs.

### How to Contribute

We welcome [issues](https://github.com/nextlinux/circleci-orb-govulners/issues) to and [pull requests](https://github.com/nextlinux/circleci-orb-govulners/pulls) against this repository!

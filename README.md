# Windows SDK
A set of resources for getting started building on CircleCI in a native Windows platform. 

## What are Windows pipelines?
Historically, CircleCI has supported `docker`, `machine`, and `macos` [executors](https://circleci.com/docs/2.0/configuration-reference/#docker--machine--macosexecutor).

With the introduction of our new Windows `resource_class` values, is now possible to run pipelines in a native Windows platform. 

## Getting started
### Prerequisites
- [ ] Organization is using a [performance plan](https://circleci.com/pricing/usage/)
- [ ] Project has [pipelines enabled](https://circleci.com/docs/2.0/build-processing/)
- [ ] User has [a zesty desire to test a product in beta](https://giphy.com/gifs/dance-dancing-cool-3o6ZtgnmZDZeAshxYY/tile)

### Configuration
Once the prerequisite conditions are met, set up a standard `.circleci/config.yml` file and define a machine job with a Windows `image` and `resource_class`. 

It is optional to define `shell` as either `powershell.exe` or `cmd.exe`. If no value is specified, the default shell will be `some-value.exe`.

```
version: 2.1

jobs:
  windows:
    machine:
      image: windows-server-2019
      resource_class: windows.medium
      shell: powershell.exe
    steps:
      - ...
```

With this executor in place, define the rest of the configuration file to meet the project needs. For more information on writing a standard CircleCI config, check out our [documentation](https://circleci.com/docs/2.0/configuration-reference/). 

> A low-effort way to get started is by creating an empty project on CircleCI consisting solely of `.circle/config.yml` file. Copy and commit our `samples/test-harness.yml` file to see a green Windows pipeline in action.

## Questions and discussion
We have a lengthy roadmap in place to port our most-loved features over to support Windows pipelines, and to add additional, Windows-specific features. We welcome your feedback on the existing beta product, and on design requests.

Please discuss Windows design in the Issues section of this repo.


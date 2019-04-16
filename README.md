# Windows on CircleCI Preview Docs
Temporary docs on how to use the pre-release preview of Windows jobs on CircleCI

## What are Windows pipelines?
Historically, CircleCI has supported `docker`, `machine`, and `macos` [executors](https://circleci.com/docs/2.0/configuration-reference/#docker--machine--macosexecutor).

With the introduction of our new Windows machine jobs, is now possible to run jobs in your pipelines in a Windows environment.

## Getting started
### Prerequisites
- [ ] Your organization is using a [performance plan](https://circleci.com/pricing/usage/)
- [ ] Your project has [pipelines enabled](https://circleci.com/docs/2.0/build-processing/)

### Configuration
Once the prerequisite conditions are met, set up a standard `.circleci/config.yml` file and define a Windows executor. The preview orb provides a powershell `shell` by default.

To view the source code for our preview orb, install our CLI tool and run:

```bash
circleci-cli orb source sandbox/windows-tools@dev:preview
```

Here is how the `.circleci/config.yml` file would look like with a Windows executor:

```YAML
version: 2.1

orbs:
  win: sandbox/windows-tools@dev:preview

jobs:
  build:
    executor: win/preview-default
    steps:
      - checkout
      - run: echo 'Hello, Windows'
```

With this executor in place, define the rest of the configuration file to meet the project's needs. For more information on writing a standard CircleCI config, check out our [documentation](https://circleci.com/docs/2.0/configuration-reference/). 

> A low-effort way to get started is by creating an empty project on CircleCI consisting solely of `.circle/config.yml` file. Copy and commit our `samples/test-harness.yml` file to see a green Windows job in action.

### Shells

There are three shells that you can use to run job steps on Windows: PowerShell, Bash or Command. You can specify the required shell as a parameter of the executor, and you can also override the shell per step.

```YAML
version: 2.1

orbs:
  win: sandbox/windows-tools@dev:preview

jobs:
  build:
    executor:
      name: win/preview-default
      shell: bash.exe
    steps:
      - checkout
      - run: ls -lah
      - run:
          command: ping circleci.com
          shell: cmd.exe
      - run:
          command: echo 'This is powershell'
          shell: powershell.exe
```


          

## Questions and discussion
We have a lengthy roadmap in place to port our most-loved features over to support Windows pipelines, and to add additional, Windows-specific features. We welcome your feedback on the existing beta product, and on design requests.

Please discuss Windows design in the Issues section of this repo.

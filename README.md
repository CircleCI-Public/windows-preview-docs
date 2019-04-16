# Windows on CircleCI Preview Docs
Temporary docs on how to use the pre-release preview of Windows jobs on CircleCI.

## Prerequisites to try out Windows on CircleCI

- [ ] Your organization is using a [performance plan](https://circleci.com/pricing/usage/)
- [ ] Your project has [pipelines enabled](https://circleci.com/docs/2.0/build-processing/)

## Please Read This Before Proceeding

* Windows support on CircleCI is currently *in the preview phase*. This is *not* production-ready software. Please do not rely on the Windows support for your production needs right now.
* As this is preview software, some product functionality has not yet been implemented. Some parts of the product might have bugs.
* The functionality not yet available on Windows includes:
	* the `deploy` step
	* Docker support
	* Workspaces
	* SSH into the job
* You might see increased spin-up times for Windows jobs during the pre-release phase.
* We are not ready to open up Windows support to the wider public yet, so please only use CircleCI for Windows in the *private repos in your org*. Please *do not* use CircleCI for Windows in open source repositories, and please do not share the examples of your Windows config outside your organization.
* Please do not more than 2 Windows jobs concurrently during the preview phase.
* Windows jobs are charged at 40 credits/minute. Please keep in mind that the pricing for Windows jobs can change in the future.

## What are Windows pipelines?
Historically, CircleCI has supported `docker`, `machine`, and `macos` [executors](https://circleci.com/docs/2.0/configuration-reference/#docker--machine--macosexecutor).

With the introduction of our new Windows machine jobs, is now possible to run jobs in your pipelines in a Windows environment.

## Getting started

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

### Frequently asked questions

> What exact version of Windows are you using?

We use Windows Server 2019 Datacenter Edition, the Server Core option.

> What is installed on the machine?

We install Git, Chocolatey, and 7zip. We don’t install any other dependencies at the moment.

If you need other software installed on the Windows machines, please open a GitHub issue and explain what software do you need, which versions of it do you use, and why is it important to you to have this software pre-installed in the machines.

> What is the machine size?

The Windows machines have 4 vCPUs and 15GB RAM.

> Did you implement any I/O optimizations?

Not right now. If there are any specific optimizations in mind that would be useful, please let us know in the GitHub issues.

> Can I use Powershell in my CircleCI config? 

Yes. The `win/preview-default` executor includes Powershell as the default shell. So in [this example](https://github.com/CircleCI-Public/windows-preview-docs/blob/master/samples/test-harness.yml#L13), the steps are being run as Powershell commands.

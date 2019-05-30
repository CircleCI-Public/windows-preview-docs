# Windows on CircleCI Preview Docs
Temporary docs on how to use the pre-release preview of Windows jobs on CircleCI. We will publish these docs in the main Docs section of our website soon.

## Prerequisites to try out Windows on CircleCI

- [ ] Your organization is using a [Performance plan](https://circleci.com/pricing/usage/) or is on a Performance plan trial
- [ ] Your project has [pipelines enabled](https://circleci.com/docs/2.0/build-processing/)

## Requesting access to Windows Preview

If you would like to request access to Windows preview, please do so [in this Google Form](https://docs.google.com/forms/d/e/1FAIpQLSfspug2MP0eTK8eRC1_FpiDQzNHkk8a36fflN_za29CwGzGoQ/viewform?usp=sf_link), and we’ll get in touch as soon as possible.

## Please Read This Before Proceeding

* Windows support on CircleCI is currently **in the preview phase**. This is **not** production-ready software. Please do not rely on the Windows support for your production needs right now.
* As this is preview software, some product functionality has not yet been implemented. Some parts of the product might have bugs.
* The functionality not yet available on Windows is:
	* the `deploy` step
	* Docker support
	* Remote Docker
	* Docker Layer Caching
* The standard functionality like caching, workspaces, SSH into the build is available for Windows jobs today. If you see any issues when using these features, those are probably bugs. Please let us know if you find those.
* You might see increased spin-up times for Windows jobs during the pre-release phase.
	* This is temporary while we prepare our systems for production load.
* During preview, we might need to turn off all Windows jobs for a period from a few minutes to a few days. This is very unlikely, but might be necessary if we need to perform infrastructure upgrades.
* If you are on a **Performance plan**, Windows jobs are charged at 40 credits/minute. If you are on a **Performance trial**, you won’t be charged. Please keep in mind that the pricing for Windows jobs can change in the future.

## What are Windows jobs?

Historically, CircleCI has supported `docker`, `machine`, and `macos` [executors](https://circleci.com/docs/2.0/configuration-reference/#docker--machine--macosexecutor).

With the introduction of our new Windows machine jobs, is now possible to run jobs in your pipelines in a Windows environment.

Windows jobs run in dedicated VMs, similar to the `machine` executor. A VM gets created for your Windows job, and gets destroyed once the job finishes.

## Getting started

### Hello, World! with the Windows Orb

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

Just paste this snippet into the `.circleci/config.yml` file in your Windows project to run a `Hello, Windows!` example.

### The Windows orb

The Windows Orb (the `sandbox/windows-tools@dev:preview` string) is the easiest way to get started with Windows. It provides a default set of options that you can customise. 

To view the source code for our preview orb, [install our CLI tool](https://circleci.com/docs/2.0/local-cli/#installation) and run:

```bash
circleci-cli orb source sandbox/windows-tools@dev:preview
```

[Learn more about Orbs here.](https://circleci.com/orbs/)

### Configuration

Once the prerequisite conditions are met, set up a standard `.circleci/config.yml` file and define a Windows executor. The preview orb provides a powershell `shell` by default.

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

There are three shells that you can use to run job steps on Windows:

* PowerShell (default in the Windows Orb)
* Bash
* Command

You can configure the shell at the job level or at the step level. So you can mix Bash and Powershell in the same job.

If you’d like to use Bash or Command instead of Powershell, add a `shell:` argument in the `executor:` section at the job level or in the step declaration:

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

### Reporting security issues

We take the security of your applications seriously. In addition to our extensive internal testing, we have gone through an external security audit for the Windows platform before making it available to the preview users.

If you believe you’ve found a security issue in CircleCI for Windows, please follow the instructions on this page to report the issue:

[https://circleci.com/security/](https://circleci.com/security/)

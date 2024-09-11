# Self-host a GitLab Runner

By default, GitLab offers a certain quota of CI time for free (if you created your account after May 17, 2021, [you may need to provide a credit card or phone number](https://about.GitLab.com/blog/2021/05/17/prevent-crypto-mining-abuse/)). However, contributing to large projects, especially multiplatform projects, can quickly eat through that quota.

CI/CD runs on machines called Runners. The free CI minutes GitLab offers are run on GitLab's Shared Runners. Of course, you can pay to increase your quota. You can also install a Runner on your own machine.

If you contribute often, you may want to install the Runner on a server or VPS than is on 24/7—but if you contribute once in a while, you may find sufficient to install the runner on your primary workstation.

The runner works by polling the GitLab API for new work, and can only accept work from projects you personally authorized it to. Since open source is organized in [forks](contributing/1-forks.md), each contributor has admin rights on their own repository, and no rights on any other, so other users cannot execute code on your runner.

This guide aims to provide a reliable process to set up your own runners on your personal fork of OpenSavvy projects.

## General runner profiles

OpenSavvy projects use three runners:

- A general-purpose runner.
- A macOS runner for multiplatform projects that support macOS or iOS.
- A privileged runner for building Docker images.

The general-purpose runner is required to contribute to any OpenSavvy projects. The other runners may or may not be required. All these runners can be installed on the same machine, if it fits each of their requirements. 

## Installing GitLab runners locally

Please refer to [this page](https://docs.gitlab.com/runner/install/) to find the installation process for GitLab runners on your system.

## Runner setup

Note that the conventions described below match those used for shared runners, thus ensuring compatibility with them.

For more detailed information, you can refer to the [official GitLab runner registration documentation](https://docs.gitlab.com/runner/register/index.html).

### General-purpose runner

In the drop-down menu found on the left of your fork, navigate to **Settings > CI/CD**.
On the following page, click the *Expand* button of the *Runners* section.

Using the *New project runner* button, create a runner with the *Run untagged jobs* checkbox of the *Runners* section checked, with its description set to "general-purpose".
After the runner is created, copy the provided **registration token**.

In your local terminal, run the following command:

```shell
sudo gitlab-runner register -n \
  --url https://gitlab.com/ \
  --registration-token REGISTRATION_TOKEN \
  --executor docker \
  --description "general-purpose" \
  --docker-image "alpine:latest" 
```

substituting the appropriate `REGISTRATION_TOKEN`, to register the general-purpose runner on your system.

### Privileged runner

In the drop-down menu found on the left of your fork, navigate to **Settings > CI/CD**.
On the following page, click the *Expand* button of the *Runners* section.

Using the *New project runner* button, create a runner with the "docker" tag under the *Tags* list, with its description set to "docker-in-docker".
After the runner is created, copy and paste the provided **registration token**.

In your local terminal, run the following command:

```
sudo gitlab-runner register -n \
  --url https://gitlab.com/ \
  --registration-token REGISTRATION_TOKEN \
  --executor docker \
  --description "docker-in-docker" \
  --docker-image "alphine:latest" \
  --docker-privileged \
  --docker-volumes "/certs/client"
```

substituting the appropriate `REGISTRATION_TOKEN`, to register the privileged runner on your system.

!!! warning
    This runner requires [privileged Docker execution](https://docs.docker.com/engine/containers/run/#runtime-privilege-and-linux-capabilities). This allows a running container (in this situation, a CI/CD job tasked with building a Docker image) to communicate with the host's Docker daemon. There are various ways in which such a container can escape and possibly take over the host.

    We recommend either using a virtual machine to encapsulate the runner, or only enabling this runner on projects you absolutely trust.

### macOS runner

!!! info ""
    This runner can only be set up on a macOS device.

Start by following the [official documentation](https://docs.gitlab.com/runner/configuration/macos_setup.html) until the "Create and register a project runner" section.

In the drop-down menu found on the left of your fork, navigate to **Settings > CI/CD**.
On the following page, click the *Expand* button of the *Runners* section.

Using the *New project runner* button, create a runner with the "saas-macos-medium-m1" tag under the *Tags* list, with its description set to "docker-in-docker".
After the runner is created, copy and paste the provided **registration token**.

In your local terminal, run the following command:

```shell
sudo gitlab-runner register -n \
  --url https://gitlab.com/ \
  --registration-token REGISTRATION_TOKEN \
  --executor shell \
  --description "macos" \
  --docker-image "alpine:latest" 
```

substituting the appropriate `REGISTRATION_TOKEN`, to register the general-purpose runner on your system.

!!! warning
    This runner requires shell access to interact with XCode.

## After setup

### Activating runners

You can use `systemctl start gitlab-runner` to activate the gitlab-runner service, which will start up the registered runners.

For convenience, it is recommended that you also run `systemctl enable gitlab-runner` once after configuration; this way, your runners will start automatically when booting up your system.

### Job concurrency

In the gitlab-runner `config.toml` file (found by default in `/etc/gitlab-runner/config.toml`), you can change the `concurrent` property to define how many jobs are allowed to run at once.
Higher concurrency lets your pipelines run faster; however, it also adds to resource consumption.

You can pick the value that works best for you, based on your resources and the length of the pipelines you need to run.

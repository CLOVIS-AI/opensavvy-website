# Forks and why we use them

Before contributing to an open source project, we need to prepare our workstation.
This tutorial will walk you through the different steps.

The approach presented here is standard for GitHub, GitLab and other similar services.
When the maintainer of a project does not specify something else explicitly, it is safe to use this tutorial to configure everything.

We expect readers of this tutorial to have basic knowledge of Git, and how to use a terminal (opening a terminal, typing commands…). 

The following software is needed:

=== "Linux"
    You will need to install `git` and `ssh`. It is likely that they are already installed by default. If they aren't, they should be easily installable through the distribution's package manager.

    [Official documentation link](https://git-scm.com/download/linux).

=== "MacOS"
    Install Git in one of the ways described in the [official documentation](https://git-scm.com/download/mac).

=== "Windows"
    On Windows, we recommend enabling the [WSL](https://learn.microsoft.com/en-us/windows/wsl/install), and then following one of the approaches for Linux.

    If you cannot use the WSL, visit the [official documentation](https://git-scm.com/download/win).

## The hosting platform

Git is software that runs on your own machine to manage versions of a project.
Most people use a hosting platform, like [GitLab](https://about.gitlab.com/) or [GitHub](https://github.com/), on which they make their projects available.

This article works for both GitLab and GitHub. It may or may not be accurate for other services.

You will need to create an account on the platform where the project is hosted: if the project is hosted on GitLab, you need a GitLab account on the same instance.

## SSH configuration

> The steps in this section need to be executed once on each computer you use.

SSH is a protocol used by computers to communicate between each other securely. It is based on cryptographic key pairs: both computers must know the public key of each other. The hosting platform needs to know which public key you will be using, to link it to your account.

To list your keys, open a terminal, and type:
```shell
$ ls ~/.ssh
```
You should see a list of files starting with `id_`, half of which ending with `.pub`. For example, `id_rsa` and `id_rsa.pub`.
If the command fails, or if you see no files following this pattern, run this command then retry the previous one:
```shell
$ ssh-keygen
```

You should now have a key pair generated on your machine. You will need to share the public key with the hosting platform: run the following command by replacing the `id_rsa.pub` part with exactly how your file is called (if you have multiple files ending in `.pub`, select one of them):
```shell
$ cat ~/.ssh/id_rsa.pub
```

At this point, the terminal should print something similar to this (it may be longer or shorter depending on the key type and size, and may be cut on multiple lines):
```shell
ssh-rsa AAAAB3NzaC1yc2EAAAADAQABAAABAQDALs0aKq6TajC5Dxjotf3u6yXfxrXuOCdf4O+w7v1gTonqcm2G39FZaQAXA3NvWuJjRYQSNcdnUZxbNLwxRjk/Afz4uFerMHMw16s5FwBxh9wxi/uZYfnPLKIB6VRI9Psdt4iFGR6+U7ddgazeiEz9Zv6Tw5zZAvOo8UBg2NoIN6g70BuDlZfYZwqlEFWHQQXD2h59bgZJlhl7c8zbD+WN+U14PZmA3OYvTRYQehr+sz1ZQIU3FUnBNHorKpBGTQgkeMtZEAvw2HxuGlfurHu3COgNdnZqoabmV04PVMBpU4nTFh6pzYGByqo44KHyHcHqGCxsnUea/E/nYl7WQzR3 an.email@address
```

Copy the entire contents. Visit your hosting platform's SSH settings, paste the contents of the file in the "Key" text area. The "Title" is useful to recognize which key comes from which computer, when you use multiple.

=== "GitLab"
    Save your key [here](https://gitlab.com/-/profile/keys).

    To check that it worked, use the following command:
    ```shell
    $ ssh -T git@gitlab.com
    ```

=== "GitHub"
    Save your key [here](https://github.com/settings/ssh/new).

    To check that it worked, use the following command:
    ```shell
    $ ssh -T git@github.com
    ```

Your computer is now successfully configured to authenticate your actions against the hosting platform.

## Forking a project

> The steps in this section need to be executed once for each project you want to contribute to.

Open source projects encourage external contributions. However, allowing anyone to make changes to the project would be dangerous! Instead, anyone is allowed to create a copy of the project, edit it as they want, and present their modifications to the original authors for approval: we call these copies "forks".

Forks are great for the original authors because they ensure contributors can edit anything without security risks, and they're great for contributors because they have complete control over the fork, so you are never limited by lack of rights to do certain things.

!!! warning
    Before making any kind of change to a project, ensure you are allowed to by the license. You can usually find it in the file `LICENSE.txt` or `README.md` in the project. If you cannot find a license, making changes to the project is likely illegal, even if the hosting platform lets you do it. To learn more about licenses, read [our dedicated article](../licenses.md).

To fork a project, simply press the "fork" button on the main page of the project on its hosting platform. In general, it is considered polite to keep the same visibility as the original project (if the original project is public, make the fork public as well). Again, see the license for the exact restrictions, if any.

## Cloning a project

> The steps in this section need to be executed once for each project you want to contribute to, on each machine you want to contribute from.

Now that you own a fork, there are multiple ways to contribute to a project. One of them is to make changes using the hosting platform's online editor. This is usually convenient for small changes that affect text, because no complex tools are necessary. When making larger changes, we usually prefer having access to the repository on our own machine.

If you've used Git before, you know this is called "cloning" a repository: the entire repository is copied to your machine. However, we have two versions of the repository: the original one, and your fork; but we can only specify a single URL when cloning.

Git is a _decentralized_ versioning system. This means all copies of the repository are completely autonomous. It is possible to edit a local copy of a repository with no network access, without needing any specific rights from the other copies. Any copy is a complete backup of the original, and can be used to reconstruct the original if it ever disappears. There is no difference between the original repository and all its clones.

Git's `clone` command is actually just an alias for a doing a few other commands at once: it creates an empty local repository (`git init`), configures it to connect to a remote repository (`git remote add`), downloads everything available on it (`git fetch`) and finally places the most recent version in your files. Now that we know this process, we can have as many remotes as we want, to access as many copies of the project as we want.

First, we clone the original repository. The URL is available under the "clone" button on the project's main page. Make sure to select the SSH URL, not the HTTPS URL (HTTPS requires you to enter your password each time you want to do any operation, and is less secure than SSH). Taking this repository as example;
```shell
$ git clone git@gitlab.com:opensavvy/website.git
```
Now, enter the directory that was created (`cd website` in this case). When cloning the project, Git configured the project to synchronize with the remote repository we provided. By default, Git called the remote `origin`. In closed source projects, there is rarely more than one remote, so it's usually fine as it is. However, we will be having multiple remotes, and remembering which one is the `origin` may be confusing. It is recommended to rename it to something clearer, for example `upstream`, or the name of the project namespace (here, `opensavvy`). For example:
```shell
$ git remote rename origin opensavvy
```

> The name can be anything you like, but it's easier to type if it's a single word without case (`example`) or multiple words separated by dashes (`a-longer-example`).

Now, we can add our fork as a remote too. Go to your fork's page, and copy its SSH clone URL. Decide on a name for it, then add it as a remote. Again, taking this repository as example, assuming you want to call your fork "mine":
```shell
$ git remote add mine git@gitlab.com:YOUR_GITLAB_USERNAME_HERE/website.git
```

At this point, your local repository is configured to access both the original repository and your fork. Sometimes, you may want to access other contributors' in-progress code. To do this, simply add their fork as a remote too.

As a last note, the `git remote` command does nothing more than configuration: it does *not* synchronize the local project with the configured remote. To synchronize the project with all configured remotes, use:
```shell
$ git fetch --all
```

The configuration is now done, everything from here is regular Git usage. You can use `git branch` to create branches, `git switch some_branch_name` to change your local files to match the `some_branch_name` branch on whichever remote it exists, etc.
To start making changes, see the [next part of this tutorial](2-changes.md).

# The AUR Template

An all-in-CI AUR helper.

## What is the AUR?

When using [ArchLinux](https://archlinux.org/), we often need to reach out to the vast [ArchLinux User Repository](https://wiki.archlinux.org/title/Arch_User_Repository) (AUR), a non-moderated repository of installation scripts for almost all software that exists.

In particular, since the AUR only contains installation scripts and not the software themselves, users can provide scripts to install closed source or restrictive software that doesn't allow redistribution of source or binaries.

However, since the AUR is not moderated, users should always verify that the scripts extract sources or binaries from trusted locations.

## AUR helpers

Scripts stored in the AUR generate binary packages, which can then be installed using Pacman. Many helpers exist to automate the download, verify the scripts, generate packages, install cycle. Most of them are based on generating the packages on your machine, which may risk infection by malicious packages.

Installing the same software on multiple machines can also be quite inconvenient.

## A CI-based AUR helper

Instead, we introduce the AUR GitLab Template: **an AUR helper that lives entirely in a GitLab repository**. Whitelist the packages from the AUR that you trust, and they are automatically built by CI and exposed to your machines. You can keep the repository private to avoid license constraints around redistribution. You can then add your GitLab repository as a Pacman repository and upgrade your systems as if they were an additional official repository.

More importantly, whenever a new version of a package is released, a merge request is created so you can review all the changes and ensure nothing malicious has been added. You can also patch the scripts to suit your specific needs.

<div class="grid cards" markdown>

- [Repository](https://gitlab.com/opensavvy/system/aur-template)
- Licensed under **Apache 2.0**

</div>

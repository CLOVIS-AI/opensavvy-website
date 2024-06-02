# Packager

As we've worked with multiple package managers on multiple distributions, it's often hard to remember the exact chain of commands to perform a simple task.

How do you upgrade all installed packages?

=== "Debian with APT"

    ```shell
    apt update
    apt upgrade
    ```

=== "NPM"

    ```shell
    npm update -g
    ```

=== "ArchLinux with Pacman"

    ```shell
    pacman -Syuu
    ```

=== "And more…"

    Each package manager has a different syntax, and you have to remember them all.

Instead, we introduce Packager:

```shell
packager upgrade
```

Packager scans your system to see which package managers are available, interactively asks you which one you want to use, and generates the correct commands to run, asking you for options along the way.

**Instead of remembering all commands for all distributions and ecosystems, just remember: `install`, `upgrade`, `remove` and `search`.**

<div class="grid cards" markdown>

- [Website](https://opensavvy.gitlab.io/system/packager/docs)
- [Repository](https://gitlab.com/opensavvy/system/packager)
- Licensed under **AGPL 3.0**
- **Easily** add support for other package managers

</div>

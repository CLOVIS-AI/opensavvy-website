# Create a GitLab project

This document describes how to configure GitLab when creating a new project that follows the OpenSavvy conventions for contribution.

All projects hosted on the OpenSavvy group must follow them.

!!! info ""
    If you want to contribute to an existing project, you do not need to go through these steps. Instead, see [the forking tutorial](contributing/1-forks.md).

## Project configuration

### Settings → General

**Naming, topics, avatar**

First, add a project description. If possible, add a project avatar too.

**Visibility, project features, permissions**

Untick "Users can request access" (users can [fork the project](contributing/1-forks.md) to contribute modifications).

### Settings → Repository

**Branch defaults**

The default branch should be called `main`. Tick "auto-close referenced issues on default branch".

**Push rules**

Tick "Present pushing secret files".

**Protected branches**

`main` branch:

- Allowed to merge: maintainers
- Allowed to push: no-one
- Allowed to force-push: no
- Code owner approval: yes

**Protected tags**

Tag `*` (wildcard for "all tags"):

- Allowed to create: maintainers

### Settings → Merge requests

**Merge requests**

Merge method: every merge creates a merge commit

Merge options:
- Enable merge request pipelines and merge trains
- Do not automatically resolve threads when they become outdated
- Do show link to create a merge request from the command line
- Enable "Delete source branch" option by default
- Do not allow squashing

Merge checks:

- Enable the "pipeline must succeed" merge check. Skipped pipelines are NOT considered successful.
    - Only enable this option *after* setting up your CI! Otherwise, you won't be able to merge anything.
- Enable "all threads must be resolved"

Merge commit message template:
```text
merge: %{title}

%{description}

See merge request %{reference}
```

**Merge request approvals**

Add a rule called "Maintainers" with the owners of OpenSavvy (`@clovis-ai` and `@maximegirardet`), targeting all branches, with 1 approval required.

Approval settings:

- Enable "prevent approval by author"
- Enable "prevent approval by users who add commits"
- Enable "prevent editing approval rules in merge requests"
- Disable "require user password to approve"
- When a commit is added: remove all approvals

### Settings → CI/CD

**General pipelines**

- Enable "public pipelines"
- Enable "auto-cancel redundant pipelines"
- Enable "prevent outdated deployment jobs"
- Enable "use separate caches for protected branches"

### Settings → Packages and registries

**Cleanup policies**

Enable the cleanup policy. Run cleanup every day.

Keep these tags:

- Keep the most recent 10 tags per image name
- Keep tags matching: `^(0|[1-9]\d*)\.(0|[1-9]\d*)\.(0|[1-9]\d*)(?:-((?:0|[1-9]\d*|\d*[a-zA-Z-][0-9a-zA-Z-]*)(?:\.(?:0|[1-9]\d*|\d*[a-zA-Z-][0-9a-zA-Z-]*))*))?(?:\+([0-9a-zA-Z-]+(?:\.[0-9a-zA-Z-]+)*))?$`

Remove these tags:

- Remove tags older than 7 days
- Remove tags matching `.*`

## Repository configuration

At the end of the README, add a section called "Contribution" with the mention:
```markdown
The various ways to contribute to this project are documented [on our website](https://opensavvy.dev/open-source/index.html).
```

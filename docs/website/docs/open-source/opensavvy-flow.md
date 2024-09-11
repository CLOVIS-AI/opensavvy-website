# OpenSavvy Flow

This document describes how to manage branches and tags in a Git repository.
This document is followed by projects maintained by OpenSavvy.
We encourage other teams to follow them as well.

This flow is inspired by the other various flows (namely, [Git Flow](https://www.atlassian.com/git/tutorials/comparing-workflows/gitflow-workflow), [GitHub Flow](https://docs.github.com/en/get-started/quickstart/github-flow) and [GitLab Flow](https://docs.gitlab.com/ee/topics/gitlab_flow.html)) but was simplified based on our usage.
It is still very similar to these, so if you use any of them you shouldn't be surprised by anything in this document.
One of the major difference is that we introduce [Semantic Versioning](https://semver.org/) into the flow itself.

All references to semantic versioning in this document refer to the [version 2.0.0](https://semver.org/spec/v2.0.0.html), or any later version with the same major version number.

## Regular development

### The default branch

The default branch of the repository must be named `main`.
All modifications, no matter their kind, must not happen directly on the `main` branch.
Instead, topic branches must be created.

The default branch represents the most recent work that passed all preliminary verification in place (e.g. peer review, automated testing…).
The default branch, however, may not be considered completely stable, as some problems may still be present (preproduction / beta channel).
To the best of the maintainer's knowledge, it should be possible to deploy the default branch to production at any time without issues.

If published to end users, they must be explicitly warned that this version may not fit their usage (common warnings include, but are not limited to, the terms "beta version", "in-development version" or "early access").

The default branch may contain features that are not considered ready for production usage yet, in which case they must be gated behind an opt-in of some kind, accompanied by warnings following the norms of topic branches (see below).

The default branch should be modified only via merges of topic branches or long-lived branches, creating a new commit in the process.

- Merging should not destroy the source of the modification (e.g. no squashing).
- Merging should keep clear which commits were created directly on the default branch, and which did not (e.g. fast-forwarding the default branch on a topic branch is not recommended).
- Any modification to the default branch must not discard any commit previously in the default branch (e.g. rebasing the default branch is not permitted).

### Topic branches

A topic branch is a short-lived branch that represents in-progress work.
Topic branches may or may not exist in the same repository as the version they aim to edit (e.g. they could be in a fork of the repository).
Topic branches should be named in a few words that provide a sufficient summary of their purpose.
They should be named in kebab case (each word separated by dashes, e.g. `word1-word2-word3`).

If published to end users, they must be explicitly warned that this version has not been verified by the project maintainers (common warnings include, but are not limited to, the terms "alpha version" or "prototype").

Topic branches should accompany an issue in a tracker of some kind.
Their name should start with the ID of that issue, according to the tracker used (e.g. for GitLab, a topic branch for the issue "#2 Create users" could be named `2-create-users`).

The name of the branch doesn't need to exactly match the name of the issue, and indeed we prefer to avoid long branch names, but it needs to be similar enough that a human understands they are related (the issue number is sufficient for machines).

### Long-lived topic branches

Topic branches should be somewhat small and focused, to help reviewers check their contents in a reasonable time.
If a large modification is planned, that can be split into multiple parts, but none of the in-between parts results can be considered of enough quality to be merged into the default branch, then a long-lived topic branch may be created.

A long-lived topic branch is a branch that behaves as the default branch, without the production-readiness requirement.
Long-lived topic branches should be rare.

An example of a long-lived topic branch could be a major project rewrite which spans multiple releases without being included in any of them, as the project continues to advance in parallel.

If a long-lived branch is created to combine planned breaking changes until they are stable to be merged into the default branch, we recommend the name `next`.

## Releases

A release branch represents a previous major or minor version that we want to provide support for.
When support ends for a particular version, its release branch is deleted.

Release branches may be created at any time, from the default branch or from another release branch.
All modifications, no matter their kind, must not happen directly on a release branch.

All modifications to a release branch must start as a topic branch (short- or long-lived).
Preferably, this topic branch should be merged into the default branch or its starting long-lived branch before being integrated in a release branch.   

Topic branches must be integrated in a release branch in one of the following ways:

- Merged into the release branch, following the same rules as if merging into the default branch,
- Squashed into the release branch, only if the topic branch has already been merged into the default branch.
  In this case, the squash commit message must contain a reference to the merge commit in the default branch.

Project releases must be represented by Git tags.
These tags must annotate commits that are either in a release branch or the default branch.
The name of these tags must be a valid version according to Semantic Versioning, with the exception that build information (`+…`) must not be used.
The relationship between project stability and Semantic Versioning is detailed in the [stability](../../stability.md) document.

All project releases from the same release branch should share the same MAJOR and MINOR version.
A release branch must be named after the MAJOR and MINOR versions it is assigned to (e.g. the branch `1.2` contains the tags `1.2.0`, `1.2.1`, etc).

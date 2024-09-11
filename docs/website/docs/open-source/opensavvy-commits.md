# OpenSavvy Commit Style

This guide describes our conventions for commit messages. Commit messages are extremely important to convey context about a change and why it was made. These conventions focus on making them informative but still short.

- Maintenance becomes simpler, as it is easy to know what changed what.
- Users can quickly find changes interesting to them.
- History modification (rebase, cherry-pick…) is easier.
- Changelogs can be [automated](https://gitlab.com/opensavvy/system/dotfiles/-/blob/main/git/kotlin/changelog.main.kts).

Here are a few examples of good commits:

```text
docs(website): Fix typos
```

```text
feat(weak): Implement WeakArrayMap

JVM and JS provide a platform-specific WeakMap implementation, but Native doesn't.
To allow multiplatform usage, we implement our own implementation that is used
by default on native, and optionally available on other platforms.

See #12
```

```text
fix(cache): Fixed OutOfMemoryError when too many items are loaded at once

Loading too many items at once led to a race condition in the item loader,
which crashed the background job responsible for clearing items.
```

## Header

!!! note ""
    **type(scope): Description**

    Body

    Footer

The header is mandatory for all commits. It looks like:
```text
type(scope): Description
```

This structure emphasizes small commits: if you can't find a single type that represents the changes in this commit, or if you can't find a single scope that represents the changes in this commit, then it should probably be split into multiple commits.

In some cases, described later in this document, it is allowed to omit the scope. In those cases, use either of these syntaxes:
```text
type: Description
```
```text
type(): Description
```

### Types

!!! note ""
    **type**(scope): Description

    Body

    Footer

Commit types allow us to get information from a quick glance in the history. For example, if we know we're searching for a dependency upgrade, we know that we can skip any commit not starting with `upgrade`.

For each of these options, prefer the first listed type (e.g. we prefer the lowercase names).

| Type                      | Use when modifying…                                                                                                                             |
|---------------------------|-------------------------------------------------------------------------------------------------------------------------------------------------|
| `build`, `Build`          | Build tooling, build environment. <br/>These changes typically do not impact the user experience.                                               |
| `ci`                      | Automation: CI/CD, quality gates… <br/>These changes typically do not impact the user experience.                                               |
| `upgrade`, `Upgrade`      | Dependency management: new dependency, upgrades, downgrades, removal of dependencies…                                                           |
| `doc`, `docs`, `Docs`     | Documentation-only changes.                                                                                                                     |
| `feat`, `New`             | Something new that the user will be interested in. <br/>Backward-compatible.                                                                    |
| `fix`, `Fix`, `fixes`     | Fixing existing problems. <br/>Backward-compatible.                                                                                             |
| `perf`, `perfs`, `Update` | Performance improvements (increased speed, decreased memory usage…) that do not otherwise impact the user experience. <br/>Backward-compatible. |
| `refactor`, `Update`      | Internal changes that are meant to facilitate future development. These should not impact the user experience. <br/>Backward-compatible.        |
| `style`                   | Grammar, coding style, linting… <br/>No behavior or API should change.                                                                          |
| `test`, `tests`           | Modifications to tests (unit tests, automated tests, manual tests…) <br/>The user experience should not change.                                 |
| `breaking`, `Breaking`    | Breaking changes, backward-incompatible changes.                                                                                                |
| `merge`                   | Merge commits. Should rarely be created manually, see [our flow](opensavvy-flow.md).                                                            |

The existence of different types that apply does not mean that a commit must absolutely be split into two. In general, a commit should list the type that is most relevant to the user.

!!! example
    When creating a new feature, it is often recommended to write tests to ensure it works (and continues working in the future). In this situation, the feature itself and its tests should be committed under a single commit with type `feat`. This is because the tests by themselves have little value to someone reading the history in the future, since they are pointless without the related feature. In contrary, having the feature and its tests in a single commit is beneficial for reviewers.

    The `test` type is primarily used when adding tests for a feature that was already part of the application before the current work iteration (e.g. topic branch) was started.

!!! tip "Rule of thumb"
    If parts of a commit would be useful by themselves to someone, then those parts should be split into their own commit such that it can be `cherry-pick`'d.

### Scope

!!! note ""
    type(**scope**): Description

    Body

    Footer

The scope allows to quickly know which parts of the projects are modified. It should be coarse-grained to allow quick reading without being too precise. It shouldn't be a file name! Readers can always look at the commit details to know which exact files have changed.

The scope should always be lowercase. Prefer a single word. If it's not possible to use a single word, `lisp-case` is tolerated.

When modifying the configuration of a tool (Make, Gradle, NPM, Docker…) that is used globally in the project, the scope can be its name.

!!! example "Examples"
    ```text
    build(gradle): Change the project name to 'material-you'

    The previous name, `Material You` contained a space, which broke included builds.
    ```
    ```text
    ci(gitlab): Execute the unit tests in CI
    ```
    ```text
    build(idea): Run configuration to start the server
    ```

Otherwise, the scope should be a simple identifier to help the user know which part of the project is changed, in a general sense. The scope should NOT be a filename. As a rule of thumb, the following definitions are probably good:

- If your project has modules (e.g. Gradle subprojects, Yarn workspaces…), the name of the impacted module should be the scope.
- If your language has a concept of packages, you can extract a scope from it. For example, with packages `fr.yourproject.core` and `fr.yourproject.utils`, `core` and `utils` are valid scopes.

A commit is allowed to change things outside its scope as long as the _primary changes_ are within its scope, and everything else is a direct consequence of these primary changes.

!!! example
    If you want to rename a function in the `core` module, and that function is used in other modules, you should create a single commit using the `core` scope that includes all impacts of the rename.

    However, a refactor in a module and another identical refactor in another module should be in different commits, as both changes are independent. Both commits may have the exact same description, only differing by their scope (for example: [commit](https://gitlab.com/opensavvy/groundwork/pedestal/-/commit/1582b8252f5903c46165d05321b0011f0e48fa4e), [commit](https://gitlab.com/opensavvy/groundwork/pedestal/-/commit/0f7b4988bd76868651f7fa46e65c9b9a40e00744)).

If your changes affect the project as a whole (e.g. dependency upgrades, merge commits), you can omit the scope.
Never use multiple scopes in a single commit.

If your project is too small to have multiple scopes, you can omit the scope.

!!! example
    This website is built in a repository that contains nothing else. Therefore, any changes _must_ be about the website, so there is no need to specify a scope.

    However, tooling commits should still have their scope: `build(idea)`, `ci(gitlab)`…

### Description

!!! note "" 
    type(scope): **Description**

    Body

    Footer

When reading code, the _what_ is fairly clear. What is often lost, however, is the _why_.

The description should be short, informative, and give the _reason_ why a commit was created.

- If you fixed a problem, explain what the problem _was_ (past tense), not how you fixed it. If you need to provide additional information on implementation tradeoffs, use the commit body.
- If you improved the performance, explain in which situation the performance was improved.
- New features should emphasize use-case. For example, `feat(website): Let users disable forms` is preferred to `feat(website): Add disable buttons`.

Specifically for dependency upgrades, the reason is considered obvious ("there is a new released version, and it is compatible with our needs"). The description should be the name of the dependency followed by the new version number:
```text
upgrade: KotlinX.Coroutines 1.9.0
```
Dependency additions, removals or downgrades must still provide a reason, in addition to the dependency name and version number.

## Body

!!! note ""
    type(scope): Description

    **Body**

    Footer

The body provides a place to describe the commit in further details. It should be used when the description isn't enough to explain exactly what the problem was (for bugs), to describe implementation tradeoffs, etc.

To decide where to put information, consider:

- If it is important to **other people using the code**: it should be in the code's documentation (javadoc, kdoc…).
- If it is important to **someone editing the code**, and will continue to be important in the future: it should be a regular comment in the code.
- If it is important to **understand why a change was made in this way**, but doesn't particularly impact future decisions, it should be in the commit body.

The body should be structured using Markdown rules (empty line to delimit paragraphs, asterisks (*) to emphasize…).

!!! example
	```text
	fix(cache): Fixed OutOfMemoryError when too many items are loaded at once
	
	Loading too many items at once led to a race condition in the item loader,
	which crashed the background job responsible for clearing items.
	```

The body can be omitted if the description already perfectly conveys all there needs to be known about the context.

For breaking changes, always include a body that explains why the change was necessary.

In merge-based workflows (Git flow, GitHub flow, GitLab flow, [OpenSavvy flow](opensavvy-flow.md)…), the merge commit should have a body that gives an overall description of the topic branch.

## Footer

!!! note ""
    type(scope): Description

    Body

    **Footer**

The footer is optional, and is used to link to external resources that may be of note, or to credit other users.

Each element of the footer should be place on its own line.
An empty line may or may not be used between elements, at the author's preference.

### Link to an issue tracker

If the commit is sufficient to completely finish an issue, its trailer should contain the words `Closes` or `Fixes` followed by the issue identifier. For example, for a GitLab issue:
```text
feat(weak): Implement WeakArrayMap

JVM and JS provide a platform-specific WeakMap implementation, but Native doesn't.
To allow multiplatform usage, we implement our own implementation that is used
by default on native, and optionally available on other platforms.

Closes #12
```

Following our [workflow](opensavvy-flow.md), commits are created in topic branches that are named after an issue. The topic branch itself will close the issue when merged. Using this workflow, the merge request must contain the `Closes` footer (as described above), but individual commits do not need to.

However, commits that refer to _other_ issues than the topic branch's they are committed in must include that trailer.

To refer to an external resource without changing its state, use `See` followed by its identifier. This is commonly used to link to related issues in other trackers (e.g. link to a bug report in a library you're using to explain a workaround you implemented).

### Trailers

You can also add trailers to add more information about the commit.

| Trailer                             | Description                                                                                                                                                                                                                 |
|-------------------------------------|-----------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------|
| `Co-authored-by: Full Name <email>` | Credit someone who helped you write this commit. This is helpful when multiple people have authored a commit. Do not confuse this with the difference between [author and committer](https://stackoverflow.com/a/18754896). |
| `Signed-off-by: Full Name <email>`  | Name the person (or people) who gave you the legal right to create this commit. This is particularly useful when an employee must obtain their supervisor's approval before making a contribution.                          |

## Merge commits, merge requests

When using GitLab, a merge commit will be generated using its merge request's information. Therefore, a merge request should follow these rules:

- Its title should follow the rules for a [commit description](#description). The rest of the header (type and scope) will be generated automatically and thus shouldn't be specified.
- Its body should follow the rules of the rest of the commit ([body](#body) and [footer](#footer)).

If using GitHub, the pull request must still follow these rules, but the person merging is responsible from writing a proper commit message based on them.

## Automation

Regular expression to verify commit headers; can be used in GitLab and JetBrains products.
```regexp
(?:build|Build|upgrade|Upgrade|ci|doc|docs|Docs|feat|New|fix|fixes|Fix|perf|perfs|Update|refactor|style|test|tests|breaking|Breaking|merge)(?:\([a-z-]*\))?: .+
```

View the last scopes used in the current project:
```shell
git log --oneline --abbrev-commit | cut -d ' ' -f 1 --complement | grep --color=auto -E '^(build|Build|upgrade|Upgrade|ci|doc|docs|Docs|feat|New|fix|fixes|Fix|perf|perfs|Update|refactor|style|test|tests|breaking|Breaking|merge)\(.*\)' | sed -r -e 's/\).*$//' -e 's/^.*\(//' | awk '!x[$0]++'
```

Generate a changelog from the history: [see script](https://gitlab.com/opensavvy/system/dotfiles/-/blob/main/git/kotlin/changelog.main.kts).

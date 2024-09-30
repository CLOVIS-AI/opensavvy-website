# Making changes

!!! note ""
    This tutorial assumes you have completed the [fork configuration](1-forks.md).

## Branching strategy

Changes are made in _branches_, which are logical copies of the project, allowing to work on multiple things in parallel.

This section describes how we prefer to name our branches. If you are contributing to a project that isn't part of the OpenSavvy organization, it is likely that the naming conventions are different.

### Understanding how we structure our branches

OpenSavvy projects have four kinds of branches:

- The default branch, `main`,
- Support branches, like `1.2` or `2.0`,
- Long-lived topic branches, like `upgrade-api`,
- Feature branches, like `5-upgrade-dependencies`.

**The default branch** is the latest version of the project. It can be considered beta-quality: all automated checks have passed successfully, but manual checks may or may not have been executed yet. When a specific commit of the `main` branch has reached quality levels we deem appropriate, we create a tag on that specific commit.

**Support branches** are created when we want to provide support for previous major or minor versions (e.g. backport bugfixes). They are always of the form `MAJOR.MINOR`, whereas tags are of the form `MAJOR.MINOR.PATH`. Like `main`, they are considered beta-quality, and we tag specific commits from the branch to become full releases. They are deleted when we stop providing support for that version, but may be recreated later if we change our minds.

**Long-lived topic branches** are created when we attempt a large modification which cannot be done in a single step, and the different steps cannot be done one-by-one. They are recognizable because their name doesn't start with a number.

**Feature branches** are where changes happen. A change always starts in a feature branch, which is merged in one of the previous three types of branches when it is completed. Feature branches always start with an issue number, followed by a short description of the issue (e.g. `12-fix-macos-pipeline` or `167-upgrade-react`).

If you want to learn more about our branching strategy, read [our workflow specification](../opensavvy-flow.md). Here's the short version:

- Everything starts with an issue.
- Changes are made in a feature branch which is named after the issue.
- When the feature branch is accepted, it is merged into `main`, except if:
    - it's a backport of a feature already present in `main`, in which case it is merged in the corresponding support branch,
    - it's a part of a larger change that had to be split into multiple steps, in which case it is merged into the corresponding longed-lived topic branch.

### Create a branch

First, select the issue you are planning on working on.

!!! tip
    Avoid starting to make changes without an existing issue, except if they are so simple that making an issue would take longer (e.g. fixing typos). Complex decisions are decided in issue comments; reviewers may refuse the change if further discussion is deemed necessary.

To help you choose an issue, follow these guidelines:

- Issues marked with `~contribution::easy` are small and have enough information for someone with basic knowledge of the project. Because these tend to be small, they are less likely to require changes during review.
- Issues marked with `~contribution::medium` have enough information for someone who understands the overall structure of the project. Expect the reviewer to request changes.
- Issues marked with `~contribution::hard` are either large, or require a deep understanding of the project. Expect a strict review.
- Issues which have none of these three labels have not been written with external contribution in mind, or are too impactful for anyone but the maintainers to attempt. If you are interested in attempting them anyway, don't hesitate to ask for clarification or help.
- Ensure no one is assigned to the issue. If someone is assigned, but no progress has happened for some time, write a comment asking if the issue is available to take.
- If the issue has the `~issue::doing` or `~issue::review` labels, there probably already exists a merge request with the start of the implementation. Refer to it to know if it was abandoned.

Add a comment on the issue to mention you are attempting to do it, to avoid two people starting working on it in parallel. The comment can be something as simple as "I'm interested in implementing this". If you have any questions about contributing to the project or if you want a rough outline of what needs to be done, don't hesitate to ask in that comment.

If you haven't set up your environment following the [forks tutorial](1-forks.md), it is time to do so.
Select a name for your branch: start with the issue number, then follow with a few words to describe it, separated by dashes.
For example, `10-submitting-changes-tutorial` is a good name.

Identify which will be the target branch using the branching rules from the previous section.
If you don't know, `main` is safe default.

Finally, you need to know how you named the remote for the official repository.
If you forgot, you can run `git remote -v` to list remotes.

Now, you can create the branch:

```shell
$ git switch -c XX-BRANCH-NAME OFFICIAL-REMOTE/TARGET-BRANCH
```

!!! example
    - I'm going to implement the issue [opensavvy/wiki#10](https://gitlab.com/opensavvy/wiki/-/issues/10).
    - This project has no support branches nor experiment branches, so the target branch will be `main`.
    - When I cloned the project, I named the remote `opensavvy`.
    - I decide to name the branch `10-submitting-changes-tutorial`.

    Therefore, the command to execute is:
    ```shell
    $ git switch -c 10-submitting-changes-tutorial opensavvy/main
    ```

Now, you need to know the remote name of your fork. Again, use `git remote -v` if you forgot.
The following command will link the branch to your fork:

```shell
$ git push -u FORK-REMOTE XX-BRANCH-NAME
```

!!! example
    I named my local remote `mine`, so the command is:
    ```shell
    $ git push -u mine 10-submitting-changes-tutorial
    ```

If it all went well, GitLab should print a link saying "To create a merge request for XX-BRANCH-NAME, visit".
Open the link, and set the following information:

- The first line should say "From YOUR-FORK/XX-BRANCH-NAME into OFFICIAL-REPOSITORY/TARGET-BRANCH". If it doesn't, your fork isn't configured correctly.
- **Title:** explain what you're going to do in a few words. In most cases, the issue title is a good title.
- **Tick 'Mark as draft'** to communicate to reviewers you just started, and it's not worth looking at it for now.
- **Description:** add a few sentences explaining what choices you did and why. It's not useful to repeat the contents of the issue. The description should end with "Closes #XX" where XX is the issue number. If the branch was named correctly, it should be added automatically by GitLab.
- Leave everything to its default, and press "Create merge request".

If you're using GitHub instead of GitLab, the process should largely be the same, except that submitted changes are called "Pull requests" instead of "Merge requests".

## Making changes and creating commits

Now that we have a branch, we can make changes to the files.
Explaining how to actually create commits is out of scope for this tutorial.

Remember that your code will have to be read by the reviewers. Making small commits with a clear description is paramount to ensuring a quick and easy reviewing process. Ensuring changes are made in an order to facilitate understanding is important. Expect reviewers to ask to split large commits, or commits which contain multiple unrelated changes.

Commit messages should have the shape `type(module): Short message explaining the goal of the change`.

- The `type` can be one of:
	- `build`: Changes to the build system,
	- `upgrade`: Dependency upgrades,
	- `ci`: Changes to external automation (CI/CD, hooks…),
	- `docs`: Documentation changes,
	- `feat`: New features,
	- `fix`: Bug fixes,
	- `refactor`: Internal improvements that aren't directly visible by the user,
	- `perf`: Performance improvements which do not change the behavior,
	- `style`: Changes to the code style, typos,
	- `test`: Changes to the verification code (all kinds of tests),
	- `breaking`: Breaking changes.
- The `module` marks the area of the project in which the modification happens. In most cases, it should be a single word, or at most two words separated by a dash.
	- In Yarn projects, this is the name of the workspace.
	- In Gradle builds, this is the name of the project.
	- For `build` and `ci`, this can also be the name of the tool which is being configured (e.g. `ci(gitlab)` for GitLab CI, `build(idea)` for changes to the `.idea` folder).
	- If other areas of the project are impacted by the change, they may still be in the same commit (e.g. renaming a function in a module impacts other modules). Multiple changes spanning multiple modules should be split into a commit each.
- The description should be a small sentence which explains the goal of the change, starting with an uppercase letter, but without a terminating period.

To learn more, [read our commit style](../opensavvy-commits.md).

Since we already linked the branch to the merge request, you are free to use `git push` whenever you want to upload your changes. Pushing will run the automated tests.

## Requesting a review

When you're satisfied with your changes, it is time to notify the reviewers. First, go back to the merge request page and edit it, adding an overall summary of your changes and your goals to the description. Remove the `Draft:` prefix from the merge request title.

Now, the reviewers will look at the merge request and request changes. Most try to review as fast as possible, but you should expect a few days of waiting.

You will be notified by email when your request is reviewed. At this point, you should read the next part: [acting on review feedback](3-review.md).

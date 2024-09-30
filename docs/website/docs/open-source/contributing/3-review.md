# Successful code reviews

!!! note ""
    This tutorial assumes you have completed the [fork configuration](1-forks.md) and [created a merge request](2-changes.md).

You have waited a bit, then received an email from GitLab notifying you that the maintainers have reviewed your MR. This document describes the next part of the process.

**First, you should understand that it is rare that a merge request is accepted on the first attempt.**
This usually only happens when the change is extremely simple, or when the contributor knows the project very well.
Even in those cases, contributions usually go through at least one round of review.

**Do not be surprised if there are a lot of comments.**
Especially when you first contribute on a project, you are not yet aware of all the conventions used by the project.
It is normal for maintainers to have a lot to say on changes you make to their project.
Do not take the number of comments as some kind of critique of your knowledge or your capacity in general.
As you continue to contribute to a project, you will learn what the maintainers expect.

**This is because a contribution doesn't end when it is accepted.**
It feels as though a contribution is a quick change that is over once it is merged, but this is incomplete.
Project maintainers have to make sure it keeps working in the future—meaning they have to be as confortable with it as you, the author, are.

Maintainers have the goal of ensuring quality and overall coherence of the project, no matter how many contributors take part.
This is only possible through strict ruling of what is and isn't allowed, even if it is sometimes arbitrary. In particular, styling decisions (code style, commit style…) are very important for maintainers to feel at ease with the contribution.

You should expect maintainers to be more strict with contributions that affect the end user's view of the project.
Tutorials are especially likely to be reviewed strictly, because they are the first experience a new user has with the project; they must reflect the library and coherent with each other.

However, this should not discourage you from submitting changes to these areas. Stricter review only means that your change is that more impactful.

Now that everyone's goals are clear, let's go through different scenarii.

## A maintainer has made comments

GitLab supports two types of comments: regular comments, and threads.
When creating a comment, a user can choose either.
Replying to a comment transforms it into a thread.

This is important to understand because threads block the review process: as long as they are unresolved, merging is not allowed.
This ensures all points brought up during review are taken care of.

Not all comments require to make changes.

- Some comments are just praise for good ideas.
- Maintainers sometimes suggest the way they would have done it, but still accept the way you did it.
- Sometimes, maintainers just don't understand why something has been done in a certain way. If a comment is phrased as question, it can mean that the maintainer would have done it differently, but isn't sure that their version is better. Usually, you can either make the change, or provide more information on why you did it a certain way.

Review comments are in the domain of human communication. Especially in short-form writing, it's easy to assume the other person meant something, when they in fact did not. Avoid assuming intent from the other person. When in doubt, don't hesitate to ask for clarification: good-willed maintainers will not respond negatively to polite and honest questions.

## Making changes

### Why is the history important?

The history is one of the most useful, yet underused, Git features. The history tells the story of how something was made—and if it's clear, _why_. This story doesn't need to be what actually happened! In real life, if you were telling someone about some events that happened in parallel, you would not tell them in chronological order, you would probably group events by some kind of meaningful common theme.

The history of a contribution is similar. Most people start by making a few changes here and there to explore what is possible, and then, when they figure it out, start making the contribution proper. However, this makes the reason why changes were made hard to understand, both in the close future (e.g. during review) and in the far future (e.g. when trying to understand a bug).

Of course, if you only need to add a little thing to your contribution, it's not always worth editing the history! In some cases, pushing new commits to the branch is encouraged. Here are the most common reasons why we would ask you to edit the history:

**Merge conflicts.** If a merge conflict occurs, the cleanest solution is to change the contribution as if it was created from the latest version of the project. This way, looking at the history, the contribution was always created from the latest version, and no conflict ever occurred. This operation, “changing the base of a contribution to a new base”, is what gives the Git command to edit the history its name: `git rebase`.

**Fixing problems.** When reading through a contribution, a common source of confusion is seeing some addition which you disagree with (you may write a comment then), and later seeing another change that removes or modifies that addition (you now need to go back to your previous comments). This back-and-forth makes changes much harder to read, thus making them more likely to be misunderstood. Instead of adding new commits that revert previous changes, we directly edit the original source of the problem to remove it. For later readers, the problem never existed.

**Making the contribution easier to understand.** If you originally committed things in the order you did them, it's possible this order isn't the easiest to understand what happened. In this case, we encourage that changes be reordered to facilitate understanding.

Again, the history is a tool to help readers understand what happened. It is not useful for future readers to know the list of things that were changed between the first attempt and the version that is finally merged. What is useful, however, is to easily be able to reconstruct why changes were made and their impacts. You may have noticed that our [commit style](../opensavvy-commits.md) is written specifically for this goal as well.

!!! note ""
    Do you think this document was written in a single sitting? It wasn't: I wrote the sections out of order, and went back to previously-written sections multiple times to edit things or add new information. However, that information is useless for anyone else reading this, so I reworked the history to look like I did do it on the first try, in order of reading. If there were any errors found during review, they have been edited out so they cannot be found in the final version, even looking at the history.
    
    Of course, this only applies to the initial version of this document. When something is merged, it enters the history of the project, which cannot be changed. Changes after that are independent, and go through their own "write–edit–reorganize" cycle.

### How can I edit the history?

!!! danger
    Before we teach you how to edit the history, we must make one thing clear: **never edit the history of work shared with other people**.
    We won't go into the kinds of problems it creates here, the short version is "you get conflicts in the history itself, not just in files".

    Here, editing the history is okay, because:

    - you're working in your own fork anyway, so at worst you can only lose your own work,
    - in our workflow, we're working on short-lived feature branches which other users can look at but not edit, so it's always clear who has ownership.

    For these reasons, you'll see that if we need to make changes to your contribution before accepting it, we prefer copying it into another branch under our own repositories first, so it's clear we can safely edit it without breaking your fork.

The rest of this document is an introduction to `git rebase`. It isn't an exhaustive list of features nor use cases, if you want that, read the [official tutorial](https://git-scm.com/book/en/v2/Git-Tools-Rewriting-History) and the [official documentation](https://git-scm.com/docs/git-rebase). If you do this often, we recommend Git Machete ([CLI](https://github.com/VirtusLab/git-machete) • [IntelliJ plugin](https://plugins.jetbrains.com/plugin/14221-git-machete)).

In a few words, rebasing means taking the list of commits in a contribution, and making a copy of each of them, in the same order. When making the copy, we can change anything we want. When we're done, we update the branches to point to the copy instead of the original. Before we start, a few important things to know:

- if you make a mistake, you can abort the current rebase operation with `git rebase --abort` to go back to the state of the project just before you started (if you didn't know, this also works with `git merge --abort` or `git cherry-pick --abort`).
- if you finished a rebase and realise later that you made a mistake, you can recover the previous version of the branch with the [reference log](https://www.atlassian.com/git/tutorials/rewriting-history/git-reflog).

As you may know, `git pull` is not a "real" Git command: it uses `git fetch` to download the remote commits, than uses one of multiple strategies to combine the remote and local changes. The default strategy is "use fast-forward if possible, use `git merge` otherwise". Because our goal is to rebase the contribution on top of the `main` branch, we will also use the `git pull` command, but with the rebase strategy.

=== "Using the terminal"
    Don't forget to replace `origin` by the name of the upstream remote (use `git remote -v` if you forgot it).

    ```shell
    git switch THE_BRANCH_YOU_WANT_TO_EDIT
    git pull --rebase=interactive origin main
    ```

=== "In JetBrains' IDEs (IntelliJ, PyCharm…)"
    JetBrains IDEs do not let you control the pull strategy, so we must first fetch, then rebase.

    - First, fetch the project with "SHIFT SHIFT" then typing "fetch".
    - Switch to the branch you want to edit.
    - Invoke the action “Rebase” with "SHIFT SHIFT" then typing "rebase".
    - In "branch or hash", type `origin/main` (replace `origin` by the name of the upstream remote).
    - In "modify options", select "`--interactive`".
    - Press "Rebase".

This will open the "todo-list". This is the list of all the commits in the contribution. You can edit the list in different ways. Once you close it, the rebase option will start. Here are a few options, and their role:

- **Pick:** this is the default. The commit will be kept as-is.
- **Drop:** the commit will be entirely removed.
- **Reword:** the commit message will be changed, but everything else will be kept the same.
- **Squash:** the commit will be squashed into the previous commit in the list. Git will ask for a new name for the resulting commit.
- **Fixup:** the commit will be squashed into the previous commit in the list. The name of the previous commit will be used for the resulting commit (the message of the fixup'd commit is lost).
- **Break** or **Pause**: the rebase will temporarily pause to let you edit the previous commit (with `git commit --amend`) or create new commits after the one selected (with `git commit`).
- You can also reorder the list to change the order of events in the history.

When it comes to editing previous commits, there are two schools of thought. Use whichever feels more natural:

- either you create a new commit which fixes the problem, then use rebase to order it right after the commit which created the problem, then squash them,
- or use rebase to pause on the commit which introduced the issue, and use `git commit --amend` to edit it to remove the problem.

Anything incoherent in the todo-list (e.g. reordering a commit that edits a file before the commit that creates the file) will lead to conflicts. If you end up with a weird conflict, don't hesitate to abort the rebase and start again. For this reason, we recommend avoiding large rebases with a lot of changes, and preferring instead multiple smaller rebases with fewer operations.

If there are no conflicts, Git will rebase the branch entirely autonomously. If there are conflicts, Git will pause and let you fix them in exactly the same manner as merge conflicts. Keep in mind that because commits are being re-recreated one-by-one, it is entirely possible, and normal, that the contents of the files during the conflict is not the latest version of your contribution. Think of it like you're rewinding the timeline and editing things before the next commits are even created. Git will autonomously update later commits to stay coherent (or conflict again if they are not). When you're done with the conflict resolution, use `git rebase --continue`. If at some point you are lost, `git status` displays the todo-list, which steps were last taken, which will be taken after continuing, and the various commands that are available such as `git rebase --edit-todo` to edit the todo-list.

Once you are done rebasing, you should observe the changes you've made, and ensure they are what you meant.

=== "Using the terminal"
    ```shell
    git log --graph --oneline ...my-fork/my-feature
    ```

    Replace `my-feature` by the name of the branch you edited, and `my-fork` by the name of your fork.

=== "In JetBrains' IDEs (IntelliJ, PyCharm…)"
    - Open the Git history (bottom-left in the "Git" tab).
    - On the right of the search bar, select the "Branch" filter, and press "Select...".
    - Write `HEAD`, then, on another line, write the name of your fork and of the branch separated by a slash (e.g. `my-fork/my-branch`).

Once you've checked that all changes match what you wanted to do, it's time to update the merge request.
Because we edited the history, a normal `git push` will fail. This is expected, and is an important protection to avoid losing work. However, in this case, we do want to replace whatever is remote by your new change.

Many people are taught that if a `push` fail, they should `pull` first to merge the remote and local changes. Some IDEs, like IntelliJ, prompt you to do it when a `push` fails. **This is a bad idea after rebasing**. If you do pull the remote changes, you will be merging the old version of your contribution with the new version of the contribution, which is definitely not what you want.

Instead, we will be forcing the remote to accept the new version. Before doing so, **ensure the remote tracking branch is the branch you want to update**.

=== "Using the terminal"
    Check which branch is tracked by the current branch:

    ```shell
    git branch -vv
    ```

    If you are happy with it, force it to accept your changes:

    ```shell
    git push --force
    ```

=== "In JetBrains' IDEs (IntelliJ, PyCharm…)"
    Start a push like you usually do. Before pushing, the IDE shows you a list of commits. Beneath the "Push" button, select "Force push".

    The IDE will open a dialog which shows the local branch and the remote branch that will be updated. Verify that it is the branch you want to update. If it is correct, continue.

If you go back to the page of the merge request, you should now see that is has been updated. You should now go through all the threads on the merge request:

- If the comment was to make a change, and you did make the change, answer with something simple like "Done". Do not click "resolve", it is the maintainer's responsibility to resolve it after they check your changes.
- If the comment was to make a change, and you did not make the change, write a comment explaining why. Asking for more precision or for help are valid answers. Do not resolve the thread.
- If the comment was a compliment on a good idea, or something similar which does not require changes, you may resolve the thread.

## And now?

Now, the maintainers will take a new look at your contribution. This will cycle for as long as necessary for the maintainers to accept the changes. Once they do, you're done: they will merge your changes.

**If you reached this point, thanks for contributing to the project, whichever it is!**

The consequence of wanting high-quality projects, is that we have to be strict on contributions, too.
We hope the process was clear enough that it wasn't too painful to follow.
If you had any issues, or think some section could be improved, do [create an issue](https://gitlab.com/opensavvy/website/-/issues/new)—or better yet, contribute a change :)

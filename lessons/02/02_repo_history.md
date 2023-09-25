# Git Practice - the history of a repository

Before you go, consider reading the [README.md](../../README.md) file at the root of the project.

## Table of Contents

- [Logs in short](#logs-in-short)

### Logs in short

Git is a version control system. To track the versions/snapshots/states, Git saves numerous data, including log journal. Git logs commits which are like bread crumbs: one follows them and therefore tracks the project.

So, a brief report of what was made so far:

```shell
╰─➤  git --no-pager log --oneline
ebb8133 (HEAD -> develop, origin/develop) lesson 01: updates
b518163 git basic lessons
4ff12de lesson 01: github remote repo
9659d20 push origin develop
4b91dca lesson 01: remote repository note
2ebfc4f (main) lesson00: introduction
```

**Memo**: the `--no-pager` option is for printing the output into the standard output stream (stdout) instead of pager program.

What is seen (assumptions are normal here):

- the commit **2ebfc4f** was made in the **main** branch with the **lesson00: introduction** message

- the last commit **ebb8133** belongs to the **develop** branch which is associated with the remote **origin/develop** branch. It is guessable that this commit is the current one because of the **HEAD** pointer (**to be checked later**, for now it is a guess).

- the commits between them belong to the **develop** branch, but it may be an erroneous assumption, so this point is not trustworthy and to be verified later in this chapter

Luckily an error in the [first lesson note](../01/01_remote_repo.md) was found, so it must be fixed and it is good for this lesson note.

The steps to take :

- `git switch -c 01_lesson` - create the branch with the name **01_lesson** and switch to it

- `git --no-pager log --oneline`

    ```shell
    ╰─➤  git --no-pager log --oneline
    ebb8133 (HEAD -> 01_lesson, origin/develop, develop) lesson 01: updates
    b518163 git basic lessons
    4ff12de lesson 01: github remote repo
    9659d20 push origin develop
    4b91dca lesson 01: remote repository note
    2ebfc4f (main) lesson00: introduction
    ```

    > Yes, the current branch is `01_lesson` and the HEAD pointer explicitly shows it

- edit the file, `git add` -> `git commit` -> `git push` it (the `--set-upstream` option won't pass silently)

Repeating the `git --no-pager log --oneline` command:

```shell
╰─➤  git --no-pager log --oneline
9328df8 (HEAD -> 01_lesson, origin/01_lesson) toc: order fixed
ebb8133 (origin/develop, develop) lesson 01: updates
b518163 git basic lessons
4ff12de lesson 01: github remote repo
9659d20 push origin develop
4b91dca lesson 01: remote repository note
2ebfc4f (main) lesson00: introduction
```

As an exercise and for future purposes, the following will be done:

- moving the **HEAD** pointer to the previous commit
- creating the branch **02_lesson** from the current (see the previous step) commit and saving the changes of this file - useful for this note and for the project as a whole: we get used to working in branches more often.

This can be done with already known command...`git switch -C <branch-name> [<start-point>]`.

The hints from the `git help switch` command:

```shell

-c <new-branch>, --create <new-branch>
    Create a new branch named <new-branch> starting at <start-point> before switching to the branch. This is a convenient shortcut for:

        $ git branch <new-branch>
        $ git switch <new-branch>

-C <new-branch>, --force-create <new-branch>
    Similar to --create except that if <new-branch> already exists, it will be reset to <start-point>. This is a convenient shortcut for:

        $ git branch -f <new-branch>
        $ git switch <new-branch>
```

So, let's try:

1. creating a new branch from a certain commit (fixed state):

    ```shell
    ╰─➤  git switch -C 02_lesson ebb8133
    Switched to a new branch '02_lesson'
    ```

2. log (HEAD points to the **02_lesson** branch, **no 9328df8 commit at all**)

    ```shell
    ╰─➤  git --no-pager log --oneline
    ebb8133 (HEAD -> 02_lesson, origin/develop, develop) lesson 01: updates
    b518163 git basic lessons
    4ff12de lesson 01: github remote repo
    9659d20 push origin develop
    4b91dca lesson 01: remote repository note
    2ebfc4f (main) lesson00: introduction
    ```

3. `git add` -> `git commit` -> `git push` as usual

In the next section we'll see the changed history and how to get it in more informative ways.

### References

- [Atlassian: Git Log](https://www.atlassian.com/ru/git/tutorials/git-log)

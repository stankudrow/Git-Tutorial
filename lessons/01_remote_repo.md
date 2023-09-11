# Git Practice - remote repository

Before you go, consider reading the [README.md](../README.md) file at the root of the project.

## Table of Contents

- [New branch](#new-branch)
- [Remote repository](#remote-repository)
- [References](#references)

## New branch

Since this note all the work will be done in a new `develop` branch.
The `main` branch wii be meant for releases and it is good to keep it clean.

First, available branches of this repository

```shell
╰─➤  git --no-pager branch --all
* main
```

The `--no-pager` option is for printing the output into the standard output stream (stdout) instead of pager program.
The `-a/--all` option is about all branches, including remote, but there are not any yet.

Now, creating a branch (`-c/--create`) and switching to it:

```shell
╰─➤  git switch --create develop
Switched to a new branch 'develop'
╭─<...> ~/Projects/git-tutorial  ‹develop*› 
```

Checking (the asterisk/star show the current branch we are at):

```shell
╰─➤  git --no-pager branch
* develop
  main
```

These branches are still local, but I want this work to be pushed into the remote repository.

## Remote repository

Time to publish this work to my GitHub account.
To achieve this goal, `git remote add <name> <url>` is helpful. It needs:

- \<url\> - the URL of a remote repository

- \<name\> - a shortcut (label) for the specified URL

This command will modify `.git/config` file for you with necessary references (the contents will be listed later).

To send data to the remote repository - `git push` command.

Steps to take:

1. `git remote origin git@github.com:stankudrow/Git-Tutorial.git` (the name of the repository will be `Git Tutorial`, GitHub replaces spaces with hyphens).

    Changes in the `.git/config` file:

    ```shell
    ╰─➤  cat .git/config 
    [core]
        repositoryformatversion = 0
        filemode = true
        bare = false
        logallrefupdates = true
    [remote "origin"]
        url = git@github.com:stankudrow/Git-Tutorial.git
        fetch = +refs/heads/*:refs/remotes/origin/*
    ```

2. `git add lessons/01_remote_repo.md`

3. `git commit -m "lesson 01: remote repository note"`

4. `git push` - failed

    ```shell
    ╰─➤  git push
    fatal: The current branch develop has no upstream branch.
    To push the current branch and set the remote as upstream, use

        git push --set-upstream origin develop
    ```

    Upstream means the relationship from local to remote.
    The inverse order is downstream.

5. aight, repeating the steps 2 and 3 + `git push --set-upstream origin develop`

    ```shell
    [main 440cd92] push forward
    1 file changed, 1 insertion(+)
    create mode 100644 .gitignore
    ```

7. `git push`

    Oops, but Git is helpful:

    ```shell
    fatal: The current branch main has no upstream branch.
    To push the current branch and set the remote as upstream, use

        git push --set-upstream origin main
    ```

8. `git push --set-upstream origin main`

    No way)

    ```shell
    ERROR: Repository not found.
    fatal: Could not read from remote repository.

    Please make sure you have the correct access rights
    and the repository exists.
    ```

    **Pay attention to the message**: *you need to have correct access rights AND the repository must exist*.

    I already have correct rights, so only repository creation is a step to take - [GitHub docs](https://docs.github.com/en/get-started/quickstart/create-a-repo).

9. finally, when the step 8 is over, `git push -u origin main` worked - now the local and remote states are synchronised.

References on Git **origin** name:

- [Stack Overflow: what-is-origin-in-git](https://stackoverflow.com/questions/9529497/what-is-origin-in-git)

- [GitHub: adding-a-local-repository-to-github-using-git](https://docs.github.com/en/migrations/importing-source-code/using-the-command-line-to-import-source-code/adding-locally-hosted-code-to-github#adding-a-local-repository-to-github-using-git)

**Done**.

## Post ~~Mortem~~... Scriptum

Some updates were added for clarifying explanations and for the next tutorial.

Also, **feel free to contribute if you like or you need some practice**. Moreover, it is a cheap way to contribute to an open-source project and get some of GitHub achievenment badges.

## References

- [Atlassian: Git Remote](https://www.atlassian.com/git/tutorials/syncing)

- [Git docs: git-remote](https://git-scm.com/docs/git-remote)

- [Stack Overflow: git-push-fatal-no-configured-push-destination](https://stackoverflow.com/questions/10032964/git-push-fatal-no-configured-push-destination)
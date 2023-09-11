# Git Tutorial - Introduction

Before you go, consider reading the README.md file at the root of the project.

## Table of Contents

- [Installing Git](#installing-git)
- [Configuring Git](#configuring-git)
- [Git init](#git-init)
- [First commit](#first-commit)

### Installing Git

Official Git documentaion explains the installation process for different systems, see this [reference](https://git-scm.com/book/en/v2/Getting-Started-Installing-Git).

In my case, I have Ubuntu 22.04.3 LTS "Jammy Jellyfish", so all I needed to do is to type `[sudo] apt install git` command.

If the installation is successful, you may check the version of `git`:

```shell
╰─➤  git --version
git version 2.34.1
```

### Configuring Git

Before the go, a good thing is to configure your `git`.
There are three (3) levels of configuration:

- `--local` - local project/directory settings saved into `.git/config` file (to be covered later in this lesson)

- `--global` - user specific configuration, data are either in `~/.gitconfig` or `~/.config/git/config` file

- `--system` - configuration for all users in the system, data are stored into `/etc/gitconfig` file

First, *name* and *email* fields which are important to be specified.
If not, `git` will remind you to set them, so, to ground the words, these fields are skipped for now.

Next, aliases for frequently used commands:

```shell
╰─➤  git config --global alias.co checkout
╰─➤  git config --global alias.br branch
╰─➤  git config --global alias.cmt commit
╰─➤  git config --global alias.st status
╰─➤  git config --global alias.sw switch
```

Then, default editor. Here `vim` will be specified, but if you don't know how to quit it, you may choose another like `emacs`, `gedit` and so forth.

```shell
╰─➤  git config --global core.editor vim
```

Then some colours and that will do:

```shell
╰─➤  git config --global color.status auto
╰─➤  git config --global color.branch auto
╰─➤  git config --global color.interactive auto
╰─➤  git config --global color.diff auto
```

I already have *name* and *email* fields at the time of writing this note, so I remove them:

```shell
╰─➤  git config --global --unset user.name
╰─➤  git config --global --unset user.email
```

Let's list the configuration:

```shell
alias.co=checkout
alias.br=branch
alias.cmt=commit
alias.st=status
alias.sw=switch
core.editor=vim
color.status=auto
color.branch=auto
color.interactive=auto
color.diff=auto
```

Done, but not forgetting *name* and *email*, it will be done later in this lesson. See the [official docs](https://git-scm.com/book/en/v2/Getting-Started-First-Time-Git-Setup) for details.

### Git init

Now, when `git` is installed and configured, it is a high time to create a project.

Steps:

- create a directory that is intended to be the project under `git` surveillance;

- step into it

The steps above are not enough.
Typing `git status` returns with an error message:

```shell
╰─➤  git status
fatal: not a git repository (or any of the parent directories): .git
```

And it is true: creating a directory does not imply being a project under `git` control. To make a git project from a directory, `git init` command is what you need.

```shell
╰─➤  git init
hint: Using 'master' as the name for the initial branch. This default branch name
hint: is subject to change. To configure the initial branch name to use in all
hint: of your new repositories, which will suppress this warning, call:
hint: 
hint:   git config --global init.defaultBranch <name>
hint: 
hint: Names commonly chosen instead of 'master' are 'main', 'trunk' and
hint: 'development'. The just-created branch can be renamed via this command:
hint: 
hint:   git branch -m <name>
Initialized empty Git repository in /<...>/Projects/git-tutorial/.git/
```

That was surprising, let's follow the advice.

```shell
╰─➤  git config --global init.defaultBranch main
╰─➤  git config --get init.defaultBranch
main
╰─➤  git branch -m main
╭─<...> ~/Projects/git-tutorial  ‹main*› 
╰─➤  
```

And now, `git status` works and displays the state of the repository.

```shell
╰─➤  git status
On branch main

No commits yet

Untracked files:
  (use "git add <file>..." to include in what will be committed)
        README.md
        lessons/

nothing added to commit but untracked files present (use "git add" to track)
```

Git has created a "hidden" directory `.git` which will contain all the information concerning your project. Listing its contents gives:

```shell
╰─➤  ls .git
branches  config  description  HEAD  hooks  info  objects  refs
```

Let's take a short look.

- `.git/config` - local (git `--local` option) configuration file. Example:

    ```shell
    ╰─➤  cat .git/config
    [core]
        repositoryformatversion = 0
        filemode = true
        bare = false
        logallrefupdates = true
    ```

- `.git/HEAD` - a file with a pointer ("reference") to the current branch. Now there is only one, `main` branch (can be though as a path or track and so on)

    ```shell
    ╰─➤  cat .git/HEAD 
    ref: refs/heads/main
    ```

- `.git/refs` - the directory with data about references (pointers) to objects, like branches and tags (tags are kind of labels, to be covered later)

- `.git/objects` - the directory with Git's object database where the states (snapshots) of the project are stored

- `.git/hooks` - the directory with script files executed when `git` commands are run

That will do for now, time for the action

### First commit

Getting the status (in short -> `-s/--short`) of the project.

```shell
╰─➤  git status -s
?? README.md
?? lessons/
```

The short format shows the status of each path as:

- `XY PATH`

- `XY ORIG_PATH -> PATH`

The status marked with `?` is untracked mening that Git knows about these files, they are in the project directory, but they are not added to the git project yet. Let's make a try to find out what `X` and `Y` statuses stand for.

Let's add only **lessons** directory with this note:

```shell
╰─➤  git add lessons/
╰─➤  git status --short
A  lessons/00_introduction.md
?? README.md
```

This file is added, but when writing this line, it is already modifed:

```shell
╰─➤  git status --short
AM lessons/00_introduction.md
?? README.md
```

And now we can guess that:

- `X` code is for the staging (index) area - the file is added with `git add` command

- `Y` code is for the project working directory - the file is modified

I may understand this wrong, so this [tutorial](https://www.tutorialspoint.com/what-is-the-short-status-in-git) dives deeper into the Git short status topic - not a bad starter.

And now, time to fix (commit) the changes:

- `lessons/00_introduction.md` is in the project state. Note, if fixing (commiting) the project state, only `git add`'ed files will be in the state. That is why I first finish this note, that `git add` changes, then;

- `README.md` is not. So when switching to this state, `README.md` file will not be present.

Typing `git commit -m "lesson00: introduction"`, where `-m` is a commit message, fixes the desired state.

## References

- [Git book](https://git-scm.com/book/en/v2)
- [W3School: Git](https://www.w3schools.com/git/default.asp)

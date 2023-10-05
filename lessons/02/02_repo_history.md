# Git Practice - the history of a repository

Before you go, consider reading the [README.md](../../README.md) file at the root of the project.

## Table of Contents

- [Logs in short](#logs-in-short)
- [Investigating the history](#investigating-the-history)
- [Change of Course](#change-of-course)
- [Cleaning the history](#cleaning-the-history)
- [What's next](#whats-next)
- [References](#references)

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

An attentive reader will notice that not everything fits together: it's as if not the whole story is output, but only part of it until the last commit in the 02_lesson branch, which is really true.

### Investigating the history

What is we switch from the **02_lesson** (this tutorial branch) to the **01_lesson** branch (the previous lesson) and run `git log`?

```shell
error: Your local changes to the following files would be overwritten by checkout:
        lessons/02/02_repo_history.md
Please commit your changes or stash them before you switch branches.
Aborting
```

It is aborted and this is a justified action. There are changes and they will be lost when switching to another branch which means switching to another snapshot/state.

- Question: how to save the progress without commiting it?

- Hint: Git has already offered you two options and one of them is not the case.

- Guess: is there a `git stash` command?

- Answer: yes -> `git help stash` -> "stash the changes in a dirty working directory away"

In short, `git-stash` shelves (stages) changes made to be restored/re-applied later when comeback is done. For details, [Atlassian: git stash](https://www.atlassian.com/git/tutorials/saving-changes/git-stash).

Typing `git stash` will take effect immediately. To unwind it, run `git stash pop` (or `git stash apply`).
Pay attention to the `git status` command to see the changes cancelled.

```shell
╰─➤  git stash
Saved working directory and index state WIP on 02_lesson: 0bc8209 lesson02: first steps
╭─<...> ~/Projects/git-tutorial  ‹02_lesson*› 
╰─➤  git status
On branch 02_lesson
Your branch is up to date with 'origin/02_lesson'.

Untracked files:
  (use "git add <file>..." to include in what will be committed)
        README.md

nothing added to commit but untracked files present (use "git add" to track)
╭─<...> ~/Projects/git-tutorial  ‹02_lesson*› 
╰─➤  git stash pop
On branch 02_lesson
Your branch is up to date with 'origin/02_lesson'.

Changes not staged for commit:
  (use "git add <file>..." to update what will be committed)
  (use "git restore <file>..." to discard changes in working directory)
        modified:   lessons/02/02_repo_history.md

Untracked files:
  (use "git add <file>..." to include in what will be committed)
        README.md

no changes added to commit (use "git add" and/or "git commit -a")
Dropped refs/stash@{0} (55045934de02c4554ad926d80f03401fcc1ebd02)
```

Difference between `pop` and `apply` options:

- `pop` will pop out the changes from the stash so they get dropped from there (**refs/stash**)

- `apply` will get the changes in effect without popping them out from the stash -> the changes are kept in the stash so they can be applied elsewhere.

Caveat: by default (!) `git stash` does not shelve the changes in untracked or ignored (to be covered later) files. To stash them as well, consider using `-u` (or `include-untracked`) or `-a` (or `--all`) option\[s\].

So, stashing is over, getting back to the `git log` inquiry.

```shell
╰─➤  git stash -u
Saved working directory and index state WIP on 02_lesson: 0bc8209 lesson02: first steps
╭─<...> ~/Projects/git-tutorial  ‹02_lesson› 
╰─➤  git switch 01_lesson
Switched to branch '01_lesson'
Your branch is up to date with 'origin/01_lesson'.
╭─<...> ~/Projects/git-tutorial  ‹01_lesson› 
╰─➤  git --no-pager log --oneline
9328df8 (HEAD -> 01_lesson, origin/01_lesson) toc: order fixed
ebb8133 (origin/develop, develop) lesson 01: updates
b518163 git basic lessons
...
```

Compare the `git log` with what is currently in the **02_lesson** branch:

```shell
╰─➤  git --no-pager log --oneline
0bc8209 (HEAD -> 02_lesson, origin/02_lesson) lesson02: first steps
ebb8133 (origin/develop, develop) lesson 01: updates
b518163 git basic lessons
...
```

The top strings are different, so we can guess that **git unwinds the history from the current commit back to root**. This sentence does not assert it, the behaviour can be more complex, but it seems so for now unless more details will be uncovered.

### Change of Course

I performed a `git stash apply` command in branch **01_lesson** and received a "terrifying" message that a conflict had appeared. So I decided to reproduce the behavior of a frightened person:

1. to pretend that nothing happened;
2. to quickly escape (switching by force (`-f`) to the desired branch (**02_lesson**));
3. to execute `git stash apply` again in the **02_lesson** branch (managed to escape)
4. to see the conflict message and get discouraged.

```shell
╭─<...> ~/Projects/git-tutorial  ‹01_lesson› 
╰─➤  git stash apply
CONFLICT (modify/delete): lessons/02/02_repo_history.md deleted in Updated upstream and modified in Stashed changes. Version Stashed changes of lessons/02/02_repo_history.md left in tree.
On branch 01_lesson
Your branch is up to date with 'origin/01_lesson'.

Unmerged paths:
  (use "git restore --staged <file>..." to unstage)
  (use "git add/rm <file>..." as appropriate to mark resolution)
        deleted by us:   lessons/02/02_repo_history.md

Untracked files:
  (use "git add <file>..." to include in what will be committed)
        README.md

no changes added to commit (use "git add" and/or "git commit -a")

╭─<...> ~/Projects/git-tutorial  ‹01_lesson*› 
╰─➤  git switch 02_lesson                                          1 ↵
lessons/02/02_repo_history.md: needs merge
error: you need to resolve your current index first

╭─<...> ~/Projects/git-tutorial  ‹01_lesson*› 
╰─➤  git switch -f 02_lesson                                       1 ↵
Switched to branch '02_lesson'
Your branch is up to date with 'origin/02_lesson'.

╭─<...> ~/Projects/git-tutorial  ‹02_lesson*› 
╰─➤  git stash apply
README.md already exists, no checkout
error: could not restore untracked files from stash
On branch 02_lesson
Your branch is up to date with 'origin/02_lesson'.

Changes not staged for commit:
  (use "git add <file>..." to update what will be committed)
  (use "git restore <file>..." to discard changes in working directory)
        modified:   lessons/02/02_repo_history.md

Untracked files:
  (use "git add <file>..." to include in what will be committed)
        README.md

no changes added to commit (use "git add" and/or "git commit -a")

╭─<...> ~/Projects/git-tutorial  ‹02_lesson*› 
╰─➤  git --no-pager log --oneline
...
```

Now there is nowhere to go and it needs to be solved somehow.

The scale of the problem:

```shell
╰─➤  git --no-pager log --graph --all --oneline
*-.   0b0d01d (refs/stash) WIP on 02_lesson: 0bc8209 lesson02: first steps
|\ \  
| | * 299586b untracked files on 02_lesson: 0bc8209 lesson02: first steps
| * c9e626c index on 02_lesson: 0bc8209 lesson02: first steps
|/  
* 0bc8209 (HEAD -> 02_lesson, origin/02_lesson) lesson02: first steps
| * 9328df8 (origin/01_lesson, 01_lesson) toc: order fixed
|/  
* ebb8133 (origin/develop, develop) lesson 01: updates
* b518163 git basic lessons
* 4ff12de lesson 01: github remote repo
* 9659d20 push origin develop
* 4b91dca lesson 01: remote repository note
* 2ebfc4f (main) lesson00: introduction
```

No way to commit this mess, also to figure out:

- git log for **02_lessons** looks fine...is it really fine or it only appears so?

    ```shell
     ╰─➤  git --no-pager log --graph --oneline      
    * 0bc8209 (HEAD -> 02_lesson, origin/02_lesson) lesson02: first steps
    * ebb8133 (origin/develop, develop) lesson 01: updates
    * b518163 git basic lessons
    * 4ff12de lesson 01: github remote repo
    * 9659d20 push origin develop
    * 4b91dca lesson 01: remote repository note
    * 2ebfc4f (main) lesson00: introduction
    ```

- extra commits from `git log --all ...` output -> highly likely `git stash` misuse.

So, a rule of thumb: a good idea is **to read the manual first**. It is similar to the principle **look before you leap** unless it is a leap of faith (in this case the faith is likely to be tested).

### Cleaning the history

Listing the stashes:

```shell
╰─➤  git --no-pager stash list
stash@{0}: WIP on 02_lesson: 0bc8209 lesson02: first steps
stash@{1}: WIP on 02_lesson: 0bc8209 lesson02: first steps
```

The commit **0bc8209** comes from the history: `0bc8209 (HEAD -> 02_lesson, origin/02_lesson) lesson02: first steps`. The recent stash is `stash@{0}` (the top of the stacck) - such a clue can be drawn from a little check:

```shell
╭─<...> ~/Projects/git-tutorial  ‹02_lesson*› 
╰─➤  git --no-pager log --graph --oneline stash@{0}
*-.   0b0d01d (refs/stash) WIP on 02_lesson: 0bc8209 lesson02: first steps
|\ \  
| | * 299586b untracked files on 02_lesson: 0bc8209 lesson02: first steps
| * c9e626c index on 02_lesson: 0bc8209 lesson02: first steps
|/  
* 0bc8209 (HEAD -> 02_lesson, origin/02_lesson) lesson02: first steps
...

╭─<...> ~/Projects/git-tutorial  ‹02_lesson*› 
╰─➤  git --no-pager log --graph --oneline stash@{1}
*   0596b0e WIP on 02_lesson: 0bc8209 lesson02: first steps
|\  
| * f2e49e4 index on 02_lesson: 0bc8209 lesson02: first steps
|/  
* 0bc8209 (HEAD -> 02_lesson, origin/02_lesson) lesson02: first steps
...
```

Let's clear the stash:

- a particular stash, e.g. `git stash drop stash@{0}`

- all stashes -> `git stash clear`

Follow the text:

```shell
─➤  git stash drop stash@{0}
Dropped stash@{0} (0b0d01d2f0f35aca2bdbad8c3fa17da7a166012c)

╰─➤  git --no-pager stash list
stash@{0}: WIP on 02_lesson: 0bc8209 lesson02: first steps

╰─➤  git --no-pager log --graph --all --oneline
*   0596b0e (refs/stash) WIP on 02_lesson: 0bc8209 lesson02: first steps
|\  
| * f2e49e4 index on 02_lesson: 0bc8209 lesson02: first steps
|/  
* 0bc8209 (HEAD -> 02_lesson, origin/02_lesson) lesson02: first steps
| * 9328df8 (origin/01_lesson, 01_lesson) toc: order fixed
|/  
* ebb8133 (origin/develop, develop) lesson 01: updates
...

╭─<...> ~/Projects/git-tutorial  ‹02_lesson*› 
╰─➤  git stash clear

╰─➤  git --no-pager log --graph --all --oneline
* 0bc8209 (HEAD -> 02_lesson, origin/02_lesson) lesson02: first steps
| * 9328df8 (origin/01_lesson, 01_lesson) toc: order fixed
|/  
* ebb8133 (origin/develop, develop) lesson 01: updates
...

```

The history got cleaner - time to stop, commit, and move on.

### What's next

In this tutorial the first problems were encountered. When this is the case, no need to panic - less chance of making things worse.

When a project is only yours, you are free to do whatever you want, but in other cases you need to be more careful and attentive - more chances to stumble and create problems for your co-workers.

Therefore, it is better to focus on conflict resolution and troubleshooting.

In the next (third) lesson, we will merge branches into the **develop** branch. Merging is a way to run into conflicts due to overlapping states to be mixed.

We will return to the `git log` command when the history gets richer to show more features of this command.

### References

- [Atlassian: Git Log](https://www.atlassian.com/ru/git/tutorials/git-log)

- [Atlassian: Git Stash](https://www.atlassian.com/git/tutorials/saving-changes/git-stash)

- [FreeCodeCamp: Git Stash](https://www.freecodecamp.org/news/git-stash-commands/)

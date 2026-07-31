# Creating Your First GitHub Repository

This guide explains how to create your first GitHub repository, connect
it to your local project, manage remotes, and push your code to GitHub.

------------------------------------------------------------------------

# Option 1: Existing Local Repository

Use this option if you already have a Git repository on your computer.

## Steps

1.  Create a **new empty repository** on GitHub.
2.  **Do not** initialize it with a README, `.gitignore`, or License.
3.  Copy the repository URL (HTTPS or SSH).
4.  Open your local project.
5.  Connect your local repository to GitHub by adding a remote.
6.  Push your code to GitHub.

### Add a Remote

A remote is simply a **label** (such as `origin`) that points to a
GitHub repository URL.

``` bash
git remote add origin https://github.com/<username>/<repository>.git
```

Verify the remote:

``` bash
git remote -v
```

Push your project:

``` bash
git push -u origin main
```

If your branch is named `master`:

``` bash
git push -u origin master
```

------------------------------------------------------------------------

# Option 2: Start From Scratch

Use this option if you haven't started your project.

1.  Create a new repository on GitHub.
2.  Copy the repository URL.
3.  Clone it to your computer.

``` bash
git clone https://github.com/<username>/<repository>.git
cd <repository>
```

Create files, then:

``` bash
git add .
git commit -m "Initial commit"
git push -u origin main
```

------------------------------------------------------------------------

# Viewing Remotes

Display configured remotes:

``` bash
git remote
git remote -v
```

If nothing is displayed, no remote has been added.

------------------------------------------------------------------------

# Adding a Remote

Syntax:

``` bash
git remote add <name> <url>
```

Example:

``` bash
git remote add origin https://github.com/<username>/<repository>.git
```

`origin` is the default name for your main GitHub repository.

------------------------------------------------------------------------

# Renaming and Removing Remotes

Rename:

``` bash
git remote rename <old> <new>
```

Remove:

``` bash
git remote remove <name>
```

------------------------------------------------------------------------

# Git Push

`git push` uploads your local commits to a remote repository.

Syntax:

``` bash
git push <remote> <branch>
```

Examples:

``` bash
git push origin main
git push origin master
```

------------------------------------------------------------------------

# Push to a Different Remote Branch

Syntax:

``` bash
git push <remote> <local-branch>:<remote-branch>
```

Examples:

``` bash
git push origin pancake:waffle
git push origin cats:master
```

Meaning: - Local `pancake` → Remote `waffle` - Local `cats` → Remote
`master`

------------------------------------------------------------------------

# What Does `git push -u` Mean?

The `-u` (`--set-upstream`) option creates a tracking relationship
between your local branch and the remote branch.

``` bash
git push -u origin main
```

After running it once, you only need:

``` bash
git push
git pull
```

------------------------------------------------------------------------

# Main vs Master

Older repositories used **master** as the default branch.

GitHub now creates new repositories with **main** as the default branch.

Check your current branch:

``` bash
git branch
```

Rename `master` to `main`:

``` bash
git branch -M main
```

Verify:

``` bash
git branch
```

Output:

``` text
* main
```

The `-M` option forcefully renames the branch.

------------------------------------------------------------------------

# Quick Reference

``` bash
git remote
git remote -v
git remote add origin <url>
git remote rename <old> <new>
git remote remove <name>
git branch
git branch -M main
git push origin main
git push origin master
git push origin pancake:waffle
git push origin cats:master
git push -u origin main
git clone <repository-url>
```

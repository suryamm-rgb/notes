# Git Diff - Complete Notes

## What is `git diff`?

The `git diff` command is used to view differences between working
directory, staging area, commits, branches, and files.

## Useful Commands

``` bash
git log --oneline
git diff
git diff HEAD
git diff --staged
git diff --cached
git diff app.js
git diff HEAD app.js
git diff --staged app.js
git diff branch1..branch2
git diff commit1..commit2
```

## `git diff`

Compares **Working Directory** with the **Staging Area**.

## `git diff HEAD`

Compares **Working Directory** with the **last commit (HEAD)**.

## `git diff --staged` / `git diff --cached`

Compares **Staging Area** with the **last commit**.

## Reading Diffs

-   `-` Removed line (old version / File A)
-   `+` Added line (new version / File B)

Example:

``` diff
- const age = 20;
+ const age = 25;
```

## Comparing Branches

``` bash
git diff main..feature
```

## Comparing Commits

``` bash
git diff commit1..commit2
```

## Summary

  Command                       Purpose
  ----------------------------- -----------------------------------
  `git diff`                    Working Directory vs Staging Area
  `git diff HEAD`               Working Directory vs Last Commit
  `git diff --staged`           Staging Area vs Last Commit
  `git diff branch1..branch2`   Compare branches
  `git diff commit1..commit2`   Compare commits

## Best Workflow

``` bash
git status
git diff
git add .
git diff --staged
git log --oneline
git commit -m "feat: message"
```

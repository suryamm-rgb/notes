# Rebasing: The "Scariest" Git Command

## Rebasing vs. Merging

When I first learned Git, I was told to avoid rebasing at all costs. But rebasing isn't actually dangerous when used correctly — it's just powerful, and power requires understanding the rules.

There are two main ways to use the `git rebase` command:

- As an **alternative to merging**
- As a **clean-up tool**

## Why Does Rebasing Have a Scary Reputation?

Rebasing rewrites commit history. If you rewrite history that other people already have (i.e., commits you've pushed and others have pulled), you can create serious conflicts for your team. That's the source of the fear — not the command itself, but using it carelessly on shared history.

## Comparing Merging and Rebasing

If the `master` branch is very active while you're working on a feature branch, merging `master` into your feature branch repeatedly creates a bunch of merge commits. This muddies your feature branch's history, making it hard to follow what actually changed.

### Merging

```bash
git switch feature
git merge master
```

This creates a merge commit that ties the two histories together. Safe, but can leave a messy, non-linear history.

### Rebasing

Instead of merging, we can **rebase** the feature branch onto `master`:

```bash
git switch feature
git rebase master
```

This moves the entire feature branch so that it begins at the current tip of `master`. All of your work is still there, but Git rewrites history — instead of creating a merge commit, rebasing creates **new commits** for each of the original feature branch commits, replayed on top of `master`.

## Why Rebase?

- Much cleaner project history
- No unnecessary merge commits
- Results in a **linear** project history that's easier to read and follow

## When *Not* to Rebase

> **Golden rule:** Never rebase commits that have been shared with others.

If you've already pushed commits to GitHub (or anywhere shared), do not rebase them — unless you are absolutely certain no one else on the team is using those commits.

## Handling Conflicts While Rebasing

*(To be continued — add notes on resolving conflicts during a rebase, using `git status` to see conflicted files, editing and staging fixes, and using `git rebase --continue`, `--skip`, or `--abort`.)*

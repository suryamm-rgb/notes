# The Ins and Outs of Stashing in Git

## Why Do We Need `git stash`?

While working on a project, you may have uncommitted changes that are not ready to be committed. Sometimes you need to:

- Switch to another branch.
- Fix an urgent bug.
- Pull the latest changes.
- Work on a different task.

Instead of creating unnecessary commits, Git provides **`git stash`** to temporarily save your changes and restore them later.

## What is `git stash`?

`git stash` is a useful Git command that temporarily saves your uncommitted changes (both staged and unstaged) and reverts your working directory to the last committed state.

```bash
git stash
```

Running this command:

- Saves your uncommitted changes.
- Removes them from your working directory.
- Lets you work on something else.
- Allows you to restore the changes later.

---

# Basic Git Stash Commands

## Save Changes

```bash
git stash
```

Or add a message:

```bash
git stash push -m "Work in progress"
```

---

## Restore and Remove the Latest Stash (`pop`)

`git stash pop` restores the most recently stashed changes **and removes them** from the stash list.

```bash
git stash pop
```

Use this when you no longer need the saved stash.

---

## Restore Without Removing (`apply`)

`git stash apply` restores the stash but **keeps it** in the stash list.

```bash
git stash apply
```

This is useful when you want to apply the same stash to multiple branches.

---

# Working with Multiple Stashes

You can create multiple stashes.

Example:

```bash
git stash
git stash
git stash
```

View all stashes:

```bash
git stash list
```

Example output:

```text
stash@{0}: WIP on main
stash@{1}: Added login page
stash@{2}: Fixed navbar
```

---

## Apply a Specific Stash

By default, Git applies the most recent stash.

To apply a specific stash:

```bash
git stash apply stash@{2}
```

This restores the selected stash without removing it.

---

# Dropping Stashes

Delete a specific stash:

```bash
git stash drop stash@{1}
```

Delete every stash:

```bash
git stash clear
```

> **Warning:** `git stash clear` permanently deletes all saved stashes.

---

# Common Git Stash Commands

| Command | Description |
|---------|-------------|
| `git stash` | Save current changes |
| `git stash push -m "message"` | Save stash with a message |
| `git stash list` | Show all stashes |
| `git stash pop` | Apply and remove latest stash |
| `git stash apply` | Apply stash without removing it |
| `git stash apply stash@{2}` | Apply a specific stash |
| `git stash drop stash@{1}` | Delete a specific stash |
| `git stash clear` | Delete all stashes |

---

# Summary

- `git stash` temporarily saves uncommitted changes.
- Use it when you need to switch tasks without committing unfinished work.
- `git stash pop` restores and removes the latest stash.
- `git stash apply` restores a stash without deleting it.
- `git stash list` shows all saved stashes.
- You can apply or delete specific stashes.
- `git stash clear` removes every stash permanently.

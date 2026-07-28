# Undoing Changes and Time Traveling in Git

## Overview

Git provides several commands to undo mistakes and move through your project's history. Since Git records every commit, you can "time travel" to previous versions of your code whenever needed.

---

# Checking Out Old Commits

`git checkout` is often described as the **Swiss Army knife** of Git because it performs many tasks:

- Create branches
- Switch branches
- Restore files
- View old commits
- Undo history

Because it became overloaded with responsibilities, Git introduced **`git switch`** and **`git restore`** as simpler alternatives.

## View Commit History

```bash
git log --oneline
```

Example:

```text
a1b2c3d Add login page
f4g5h6i Fix navbar
```

Checkout an old commit using the first 7 characters:

```bash
git checkout a1b2c3d
```

---

# Detached HEAD

After checking out an old commit, Git displays a **Detached HEAD** message.

Don't panic—this is completely normal.

You have three options:

1. Stay in Detached HEAD and inspect the old commit.
2. Switch back to your previous branch.
3. Create a new branch to continue making changes.

---

# Referencing Commits Relative to HEAD

```text
HEAD~1  -> Parent commit
HEAD~2  -> Grandparent commit
```

Example:

```bash
git checkout HEAD~1
```

---

# Discarding Changes with Checkout

Restore a file to its last committed state:

```bash
git checkout HEAD app.js
```

Shortcut:

```bash
git checkout -- app.js
```

---

# Git Restore

`git restore` is a newer Git command designed specifically for undoing file changes.

Restore a file:

```bash
git restore app.js
```

Restore from an older commit:

```bash
git restore --source HEAD~1 app.js
```

> Warning: This permanently discards uncommitted changes.

---

# Unstage Files

Remove a file from the staging area:

```bash
git restore --staged app.js
```

or

```bash
git restore --staged <file-name>
```

---

# Undo Commits with Git Reset

Reset to a previous commit:

```bash
git reset <commit-hash>
```

Example:

```bash
git reset HEAD~1
```

## Hard Reset

```bash
git reset --hard HEAD~1
```

Removes the commit and all associated changes.

---

# Undo Commits with Git Revert

`git revert` creates a **new commit** that reverses the changes made by an earlier commit.

```bash
git revert <commit-hash>
```

Unlike `git reset`, it preserves project history.

---

# Committing Changes

## New Files

```bash
git add .
git commit -m "Add Header component"
```

or

```bash
git add src/components/Header.tsx
git commit -m "Add Header component"
```

## Modified Tracked Files

```bash
git commit -a -m "Update Header styles"
```

---

# Git Reset vs Git Revert

| Git Reset | Git Revert |
|-----------|------------|
| Rewrites commit history | Preserves history |
| Moves branch pointer backwards | Creates a new commit |
| Best for local commits | Best for shared commits |

## Which One Should You Use?

- Use **`git reset`** for commits that have **not** been pushed or shared.
- Use **`git revert`** for commits that have already been pushed or shared with others.

---

# Summary

- Use `git checkout` to inspect old commits.
- Detached HEAD is safe and temporary.
- Use `git restore` to undo file changes.
- Use `git restore --staged` to unstage files.
- Use `git reset` for local history changes.
- Use `git revert` for safely undoing shared commits.

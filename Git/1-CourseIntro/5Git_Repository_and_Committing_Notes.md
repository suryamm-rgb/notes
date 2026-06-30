# Git Repository & Commit Basics Notes

## What is a Git Repository?

A Git repository (Git repo) is a workspace that tracks and manages files inside a folder.

Whenever we want to use Git with a project, application, or codebase, we need to create a new Git repository.

We can have multiple repositories on our machine, each with separate:
- History
- Files
- Changes
- Commits

---

# Our First Git Command

## git status

`git status` shows the current status of a Git repository and its contents.

It displays:
- Modified files
- New files
- Staged changes
- Repository information

```bash
git status
```

---

# Initializing a Git Repository

## git init

Before using Git, we must initialize a repository.

This is done once per project.

Initialize the repo in the top-level project folder.

```bash
git init
```

This creates a hidden `.git` folder.

Show hidden files:

```bash
ls -a
```

---

# Committing - The Most Important Git Feature

A commit is a snapshot of changes in a project.

## Basic Git Workflow

1. Work on files
   - Create files
   - Edit files
   - Delete files

2. Add changes
   - Select changes for the next commit

3. Commit
   - Save staged changes into Git history

---

# Git Flow

```
Working Directory
        |
     git add
        |
        v
Staging Area
        |
    git commit
        |
        v
Repository
```

---

# Adding Changes

`git add` stages changes for the next commit.

It tells Git to include selected changes.

Add specific files:

```bash
git add file1 file2
```

---

# Git Commit

`git commit` saves staged changes into the repository.

```bash
git commit
```

A commit message describes the changes included in the snapshot.

---

# Commit Message Shortcut

Use `-m` to write the message directly:

```bash
git commit -m "my message"
```

---

# Stage All Changes

Add all changes:

```bash
git add .
```

Then commit:

```bash
git commit -m "describe changes"
```

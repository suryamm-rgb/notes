# Git Basics – Beginner Notes

## 1. `git init`

**Purpose:** Initialize a Git repository.

- Creates a new Git repository in your current project folder.
- Git creates a hidden `.git` directory that stores the repository's history and configuration.
- Use this command when starting a new project that is not yet under version control.

```bash
git init
```

---

## 2. `git clone <repository-url>`

**Purpose:** Clone an existing repository.

- Downloads an existing repository from GitHub, GitLab, or another Git server.
- Copies the complete project, including all branches and commit history, to your local machine.
- Automatically initializes Git for the cloned project.

```bash
git clone <repository-url>
```

Example:

```bash
git clone git@gitlab.com:company/project.git
```

---

## 3. `git status`

**Purpose:** Check the current status of your repository.

Displays:

- Modified files
- Newly created files
- Deleted files
- Untracked files
- Staged files
- Current branch

```bash
git status
```

Example output:

```
modified: app.js
new file: login.js
deleted: old.css
```

---

## 4. `git add`

**Purpose:** Move changes to the Staging Area.

Before a commit, Git requires you to select which changes should be included.

Stage a single file:

```bash
git add app.js
```

Stage all files:

```bash
git add .
```

After running `git add`, the selected changes are ready to be committed.

---

## 5. `git commit`

**Purpose:** Save changes to the Local Repository.

- Creates a snapshot of the staged changes.
- Stores the snapshot in your local Git history.
- Every commit should include a meaningful message describing the changes.

```bash
git commit -m "Add user authentication"
```

---

## 6. `git log`

**Purpose:** View commit history.

Displays:

- Commit ID (SHA)
- Author
- Date
- Commit message

```bash
git log
```

Compact view:

```bash
git log --oneline
```

The latest commit is pointed to by **HEAD**.

---

## 7. `git diff`

**Purpose:** Compare differences between files.

### Compare unstaged changes

```bash
git diff
```

Shows the difference between the Working Directory and the Staging Area.

### Compare staged changes

```bash
git diff --staged
```

Shows the difference between the Staging Area and the last commit.

### Compare with the latest commit

```bash
git diff HEAD
```

Shows all changes since the latest commit.

---

# Git Workflow

```
Working Directory
        │
        │ git add
        ▼
Staging Area (Index)
        │
        │ git commit
        ▼
Local Repository (.git)
        │
        │ git push
        ▼
Remote Repository (GitHub/GitLab)
```

---

# Daily Git Commands

```bash
# Check repository status
git status

# Stage all files
git add .

# Stage a specific file
git add src/index.js

# Save changes
git commit -m "Add login validation"

# View commit history
git log

# View compact commit history
git log --oneline

# Show unstaged changes
git diff

# Show staged changes
git diff --staged

# Compare with the latest commit
git diff HEAD
```

---

# Summary

| Command                         | Purpose                      | Explanation                                                                                                                                                         |
| ------------------------------- | ---------------------------- | ------------------------------------------------------------------------------------------------------------------------------------------------------------------- |
| `git init`                      | Initialize a Git repository  | Creates a new Git repository in your current project folder by adding a hidden `.git` directory. Git starts tracking your project from this point.                  |
| `git clone <repository-url>`    | Clone an existing repository | Downloads a complete copy of a remote repository (such as from GitHub or GitLab) to your local machine, including all branches and commit history.                  |
| `git status`                    | Check repository status      | Shows the current state of your working directory and staging area. It tells you which files are modified, newly created, deleted, staged for commit, or untracked. |
| `git add <file>` or `git add .` | Stage changes                | Moves selected changes from the working directory to the staging area. Only staged changes will be included in the next commit.                                     |
| `git commit -m "message"`       | Save changes locally         | Creates a snapshot of the staged changes and stores it in your local Git history. Each commit should have a meaningful message describing the changes.              |
| `git log`                       | View commit history          | Displays the list of commits in the repository, including the commit hash (SHA), author, date, and commit message. The latest commit is pointed to by `HEAD`.       |
| `git diff`                      | Compare changes              | Shows the differences between files. By default, it compares the working directory with the staging area. It can also compare staged changes, commits, or branches. |

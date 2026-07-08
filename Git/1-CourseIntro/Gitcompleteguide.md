# Git: Complete Guide (Beginner → Advanced)

A single reference covering everything from "what is version control" to git internals and large-repo management, with explanations, **why you'd use each thing**, and practical examples.

---

# PART 1: BEGINNER

## 1.1 What is Version Control?

**What it is:** A system that records changes to files over time so you can recall specific versions later. Instead of `report_final.docx`, `report_final_v2.docx`, `report_FINAL_ACTUALLY.docx`, version control tracks every change in one place.

**Why use it:**

- See exactly what changed, when, and who changed it.
- Revert to any previous state instantly.
- Multiple people can work on the same project without overwriting each other's work.
- Experiment safely (via branches) without breaking working code.

**Types:**

- **Local VCS** – versions stored on your own machine.
- **Centralized VCS** (e.g., SVN) – one central server holds history; everyone connects to it.
- **Distributed VCS** (Git, Mercurial) – every clone has the _full_ history, not just the latest snapshot. This is why Git works offline and why it's fast and resilient.

---

## 1.2 Git Installation

**What it is:** Getting Git onto your machine so you can use it from the command line.

**Why:** You can't do anything below without it.

**Examples:**

```bash
# Debian/Ubuntu
sudo apt update && sudo apt install git

# macOS (Homebrew)
brew install git

# Windows
# Download from https://git-scm.com/downloads (includes Git Bash)

# Verify installation
git --version

# First-time setup (do this once per machine)
git config --global user.name "Your Name"
git config --global user.email "you@example.com"
git config --global init.defaultBranch main
```

---

## 1.3 Repository ("repo")

**What it is:** A folder that Git is tracking. It contains your files plus a hidden `.git` folder where Git stores all history, branches, commits, and configuration.

**Why:** The repo is the unit Git operates on — everything (commits, branches, tags) lives inside it.

**Example:**

```bash
mkdir my-project
cd my-project
git init
ls -la
# .git/   <-- this is the repository's database
```

---

## 1.3b What On Earth Is `HEAD`?

**What it is:** `HEAD` is a pointer to "where you currently are" — usually it points to a branch name, which in turn points to the latest commit on that branch. When you switch branches, `HEAD` moves with you.

**Why it matters:** Almost every Git command (`commit`, `reset`, `checkout`, `diff`) operates relative to `HEAD`. Understanding it demystifies terms like "detached HEAD."

**Example:**

```bash
cat .git/HEAD
# ref: refs/heads/main   <-- HEAD points to the 'main' branch, which points to a commit

git checkout a1b2c3d
# You are now in 'detached HEAD' state — HEAD points directly to a commit,
# not a branch. Any new commits here can be "lost" unless you create a branch:
git switch -c rescue-branch
```

---

## 1.4 `git init`

**What it is:** Initializes a new, empty Git repository in the current folder.

**Why use it:** You use this when starting version control on a brand-new project (one that doesn't already exist on GitHub/GitLab).

**Example:**

```bash
mkdir todo-app
cd todo-app
git init
# Initialized empty Git repository in /todo-app/.git/
```

---

## 1.5 `git clone`

**What it is:** Copies an _existing_ remote repository (and its entire history) onto your machine.

**Why use it:** You use this when joining a project that already exists (e.g., on GitHub), instead of `git init`.

**Example:**

```bash
git clone https://github.com/torvalds/linux.git

# Clone into a custom folder name
git clone https://github.com/user/repo.git my-folder

# Clone only the latest history (faster for huge repos)
git clone --depth 1 https://github.com/user/repo.git
```

---

## 1.6 `git status`

**What it is:** Shows the current state of your working directory: which files are modified, staged, or untracked.

**Why use it:** It's your "what's going on right now" command — run it constantly before committing so you know exactly what will be included.

**Example:**

```bash
git status

# On branch main
# Changes not staged for commit:
#   modified:   index.html
# Untracked files:
#   style.css
```

---

## 1.7 `git add`

**What it is:** Moves changes from the "working directory" into the "staging area" (a.k.a. the index) — preparing them to be committed.

**Why use it:** It lets you choose exactly what goes into the next commit, rather than committing everything blindly. This means you can group related changes into clean, logical commits.

**Example:**

```bash
git add index.html          # stage one file
git add .                   # stage everything in current folder
git add -A                  # stage everything in the whole repo
git add -p                  # interactively choose specific chunks/hunks to stage
```

---

## 1.8 `git commit`

**What it is:** Takes everything in the staging area and permanently records it as a snapshot in the repo's history, with a message describing the change.

**Why use it:** This is how you actually save a version. Good commit messages make history readable and let you (or teammates) understand _why_ a change was made months later.

**Example:**

```bash
git commit -m "Add login form validation"

# Stage + commit tracked files in one step (skips 'git add' for modified files only)
git commit -am "Fix typo in header"

# Amend the last commit (e.g., you forgot a file or made a typo in the message)
git commit --amend -m "Corrected commit message"
```

---

## 1.9 `git log`

**What it is:** Shows the commit history of the repository.

**Why use it:** To review what's been done, find a specific commit hash, understand who changed what, or track down when a bug was introduced.

**Example:**

```bash
git log                       # full history
git log --oneline             # compact, one line per commit
git log --oneline --graph --all   # visual branch graph
git log -p -2                 # show diffs for last 2 commits
git log --author="Jane"       # filter by author
git log --since="2 weeks ago"
```

---

## 1.10 `git diff`

**What it is:** Shows the exact line-by-line differences between two states (working directory vs. staging, staging vs. last commit, between commits, between branches).

**Why use it:** To review _exactly_ what you changed before committing — catch accidental changes, debug lines, or leftover console.logs.

**Example:**

```bash
git diff                  # unstaged changes vs last commit
git diff --staged         # staged changes vs last commit
git diff HEAD~1 HEAD       # diff between two commits
git diff main feature-x    # diff between two branches
git diff -- file.js        # diff for one specific file
```

---

## 1.11 `.gitignore`

**What it is:** A file listing patterns for things Git should never track — build output, dependencies, secrets, OS junk files.

**Why use it:** Keeps the repo clean and prevents accidentally committing things like `node_modules/`, `.env` secrets, or `.DS_Store`.

**Example:**

```
# .gitignore
node_modules/
dist/
.env
*.log
.DS_Store
```

```bash
# If a file is already tracked, .gitignore won't stop it — untrack it first:
git rm --cached secrets.env
```

---

## 1.12 `git restore` vs `git checkout`

**What it is:** `git restore` (newer, Git 2.23+) is a clearer, more focused replacement for some of the older, overloaded jobs `git checkout` used to do — restoring files in the working directory or unstaging files.

**Why use it:** `checkout` historically did _too many things_ (switch branches, restore files, create branches). `restore` and `switch` split those responsibilities so command intent is obvious.

**Example:**

```bash
git restore file.js              # discard unstaged changes in file.js (like old 'checkout -- file.js')
git restore --staged file.js     # unstage a file, keep the changes (like old 'reset file.js')
git restore --source=HEAD~1 file.js   # restore file.js to how it looked 1 commit ago
```

---

## 1.13 Committing With a GUI

**What it is:** Visual Git clients (GitHub Desktop, GitKraken, Sourcetree, VS Code's built-in Git panel, `git gui`/`gitk`) let you stage, commit, branch, and view diffs visually instead of via terminal.

**Why use it:** Great for visualizing diffs, staging partial changes, and reviewing history — many developers use the CLI for day-to-day speed but a GUI for reviewing complex diffs or history graphs.

**Example:**

```bash
gitk                # built-in graphical history viewer
git gui              # built-in graphical commit tool
code .               # VS Code has Git staging/commit/diff built into the Source Control tab
```

---

## 1.14 Commit Message Conventions

**What it is:** Standardized style for writing commit messages — most teams use the **imperative/present tense** ("Add feature" not "Added feature") because Git itself generates messages this way (e.g., "Merge branch...").

**Why use it:** Consistency makes `git log` easier to scan and makes automated changelog tools work correctly.

**Example:**

```
Good: "Fix null pointer exception in login handler"
Avoid: "Fixed a bug" / "Fixes bug" (inconsistent tense)

# Common convention: Conventional Commits
feat: add user authentication
fix: correct off-by-one error in pagination
docs: update README installation steps
refactor: simplify order validation logic
```

---

# PART 2: INTERMEDIATE

## 2.1 Branches

**What it is:** A branch is an independent, movable pointer to a line of development. `main` is just the default branch — you can create as many as you want.

**Why use it:** Branches let you build a feature, fix a bug, or experiment _without touching the stable code_ on `main`. This is the foundation of team workflows (feature branches, `git flow`, trunk-based development, etc.).

**Example:**

```bash
git branch                     # list branches
git branch feature/login       # create a new branch
git checkout feature/login     # switch to it
# OR, shortcut for both at once:
git checkout -b feature/login
git switch -c feature/login    # modern equivalent of checkout -b

git branch -d feature/login    # delete a merged branch
git branch -D feature/login    # force-delete (even if unmerged)
```

---

## 2.2 Merge

**What it is:** Combines the changes from one branch into another, creating a new "merge commit" that has two parents (unless it's a simple fast-forward).

**Why use it:** This is how finished feature-branch work gets integrated back into `main`. It preserves the full history of both branches — good for auditability and understanding _how_ a feature evolved.

**Example:**

```bash
git checkout main
git merge feature/login
# If feature/login has no divergence from main, this is a "fast-forward" merge.
# If both have new commits, Git creates a merge commit.

# Force a merge commit even if fast-forward is possible (keeps history explicit)
git merge --no-ff feature/login
```

---

## 2.3 Rebase

**What it is:** Replays your branch's commits _on top of_ another branch's latest commit, instead of creating a merge commit — resulting in a linear history.

**Why use it:** Cleaner, linear history that's easier to read (`git log --oneline` looks like a straight line instead of a web). Commonly used to update a feature branch with the latest `main` before merging/PR.

**Example:**

```bash
git checkout feature/login
git rebase main
# Your feature commits are now replayed on top of main's latest commit

# If conflicts occur mid-rebase:
git status                # see conflicted files
# ... fix conflicts manually ...
git add <fixed-file>
git rebase --continue
# or abort entirely and go back to pre-rebase state:
git rebase --abort
```

⚠️ **Rule of thumb:** Never rebase commits that have already been pushed and shared with others — it rewrites history and breaks collaborators' copies.

---

## 2.4 Cherry Pick

**What it is:** Applies a _single specific commit_ from one branch onto another, without merging the whole branch.

**Why use it:** Useful when you need just one fix (e.g., a hotfix) from a branch, without pulling in unrelated work — common for backporting a bugfix to a release branch.

**Example:**

```bash
git log feature/login --oneline
# a1b2c3d Fix null pointer bug
# e4f5g6h Add new UI (don't want this one)

git checkout main
git cherry-pick a1b2c3d
# Only the bugfix commit is applied to main
```

---

## 2.5 Stash

**What it is:** Temporarily shelves (saves) uncommitted changes so you can switch context (e.g., branches), then reapplies them later.

**Why use it:** You're mid-work, changes aren't ready to commit, but you urgently need a clean working directory (e.g., to switch branches or pull updates).

**Example:**

```bash
git stash                     # save current changes, revert to clean state
git stash list                 # see all stashes
git stash pop                  # reapply latest stash and remove it from stash list
git stash apply                # reapply latest stash but KEEP it in the list
git stash push -m "WIP login"  # stash with a descriptive message
git stash drop stash@{0}       # delete a specific stash
```

---

## 2.6 Reset

**What it is:** Moves the current branch pointer (HEAD) to a different commit, with three modes controlling what happens to your working directory and staging area.

**Why use it:** To undo commits locally — e.g., you committed too early, or want to uncommit/restage changes.

**Example:**

```bash
git reset --soft HEAD~1   # undo last commit, KEEP changes staged
git reset --mixed HEAD~1  # undo last commit, KEEP changes but UNstaged (default)
git reset --hard HEAD~1   # undo last commit, DISCARD all changes entirely (destructive!)

git reset <file>          # unstage a specific file (keep its changes)
```

⚠️ `--hard` permanently deletes uncommitted work — use carefully.

---

## 2.7 Revert

**What it is:** Creates a _new_ commit that undoes the changes from a specific previous commit — without altering history.

**Why use it:** Safer than `reset` for shared/public branches, because it doesn't rewrite history — it just adds a new "undo" commit. Ideal for undoing something already pushed to a shared branch.

**Example:**

```bash
git revert a1b2c3d
# Creates a new commit that reverses the changes introduced by a1b2c3d

git revert HEAD           # revert the most recent commit
git revert --no-commit a1b2c3d..HEAD   # revert a range without auto-committing
```

---

## 2.8 Tags

**What it is:** A fixed, named pointer to a specific commit — typically used to mark release points (`v1.0`, `v2.1.3`).

**Why use it:** Unlike branches, tags don't move. They're perfect for marking "this exact commit is what we shipped as version 1.0."

**Example:**

```bash
git tag v1.0                          # lightweight tag on current commit
git tag -a v1.0 -m "Release version 1.0"  # annotated tag (recommended: stores author, date, message)
git tag                               # list all tags
git push origin v1.0                  # push a single tag
git push origin --tags                # push all tags
git checkout v1.0                     # inspect code at that tag (detached HEAD)
```

---

## 2.9 Remotes: `git push`, `git pull`, `git fetch`

**What it is:** A "remote" is a version of your repo hosted elsewhere (e.g., GitHub, GitLab). These three commands sync your local repo with it.

- `git fetch` – downloads new commits/branches from the remote but does **not** merge them into your local branches.
- `git pull` – does `fetch` **+ merge** (or rebase) into your current branch, in one step.
- `git push` – uploads your local commits to the remote.

**Why use it:** This is how teams share work. `fetch` is the "safe" way to see what changed without touching your files; `pull` updates you immediately.

**Example:**

```bash
git remote add origin https://github.com/user/repo.git
git remote -v                      # list configured remotes

git fetch origin                   # see what's new, without merging
git log origin/main --oneline      # inspect remote's commits before merging

git pull origin main               # fetch + merge in one step
git pull --rebase origin main      # fetch + rebase instead of merge (linear history)

git push origin main               # upload your commits
git push -u origin feature/login   # push + set upstream tracking (lets you just type 'git push' next time)
```

---

## 2.10 Git Reflogs

**What it is:** A local, chronological log of _every_ place `HEAD` has pointed to — every commit, checkout, reset, rebase, and merge — even ones no longer visible in `git log`.

**Why use it:** `git log` shows the current history graph, but a botched `reset --hard` or rebase can seem to "delete" commits. Reflog is your safety net — those commits usually still exist until Git's garbage collector eventually cleans them up.

**Example:**

```bash
git reflog
# a1b2c3d HEAD@{0}: commit: Add validation
# e4f5g6h HEAD@{1}: reset: moving to HEAD~1
# h7i8j9k HEAD@{2}: commit: Fix typo

# Recover a commit that seems "lost" after a bad reset:
git reset --hard HEAD@{1}

# Undo an entire rebase gone wrong:
git reflog                     # find the commit hash from BEFORE the rebase started
git reset --hard <that-hash>
```

⚠️ **Limitation:** Reflogs are **local only** — not pushed to remotes, and entries eventually expire (default ~90 days for reachable, ~30 for unreachable commits).

---

## 2.11 Git Aliases

**What it is:** Custom shortcuts for long or frequently-used Git commands, defined in your Git config.

**Why use it:** Saves keystrokes and standardizes your personal workflow — e.g., `git st` instead of `git status`.

**Example:**

```bash
# Set from command line
git config --global alias.st status
git config --global alias.co checkout
git config --global alias.br branch
git config --global alias.last "log -1 HEAD"
git config --global alias.lg "log --oneline --graph --all"

# Now you can run:
git st
git lg

# Aliases with arguments (must be a shell command, prefixed with '!')
git config --global alias.unstage '!git reset HEAD --'
```

You can also edit `~/.gitconfig` directly:

```ini
[alias]
    st = status
    co = checkout
    br = branch
    lg = log --oneline --graph --all
```

---

## 2.12 GitHub Collaboration Workflows

**What it is:** The common patterns teams use to collaborate through a shared remote (usually GitHub/GitLab):

- **Feature branching** – create a branch per feature/fix, merge back via PR.
- **Fork & clone** – for open-source contributions: you copy ("fork") someone else's repo to your own account, clone _your_ fork, make changes, then open a Pull Request back to the original.
- **Pull Requests (PRs)** – a request to merge your branch into another, with a space for code review, comments, and CI checks before merging.

**Why use it:** Enables safe collaboration — nobody pushes directly to `main`; changes are reviewed first.

**Example:**

```bash
# Feature branching (same repo, team members with write access)
git switch -c feature/checkout-flow
# ...work, commit...
git push -u origin feature/checkout-flow
# Open a Pull Request on GitHub: feature/checkout-flow -> main

# Fork & clone (contributing to someone else's project)
# 1. Click "Fork" on GitHub (creates your own copy: github.com/you/repo)
git clone https://github.com/you/repo.git
cd repo
git remote add upstream https://github.com/original-owner/repo.git
git fetch upstream
git merge upstream/main            # keep your fork updated
git switch -c fix/typo
# ...make changes, commit, push to YOUR fork...
git push origin fix/typo
# Then open a PR from your fork's branch into the original repo on GitHub
```

---

## 2.13 Tags & Semantic Versioning

**What it is:** A convention for version numbers: `MAJOR.MINOR.PATCH` (e.g., `v2.4.1`) — MAJOR = breaking changes, MINOR = new backward-compatible features, PATCH = bug fixes.

**Why use it:** Combined with Git tags, it gives every release a clear, predictable, machine-readable identity that tools (npm, Docker, CI/CD) can rely on.

**Example:**

```bash
git tag -a v1.0.0 -m "Initial stable release"
git tag -a v1.1.0 -m "Add search feature (backward-compatible)"
git tag -a v1.1.1 -m "Fix search pagination bug"
git tag -a v2.0.0 -m "Breaking change: new API response format"

git push origin --tags
```

---

## 2.14 GitHub Pages, Gists & Markdown READMEs

**What it is:**

- **GitHub Pages** – free static site hosting directly from a repo (perfect for docs, portfolios, project demos).
- **GitHub Gists** – lightweight, shareable snippets of code (mini-repos with their own version history).
- **Markdown README** – `README.md` is the file GitHub renders on a repo's homepage — the "front door" documentation for any project.

**Why use it:** Free hosting/documentation infrastructure directly tied to your code — no separate services needed.

**Example:**

```bash
# GitHub Pages: enable in repo Settings > Pages > deploy from 'main' branch, '/docs' folder or gh-pages branch
git switch -c gh-pages
git push origin gh-pages
# Site becomes live at: https://username.github.io/repo-name/

# Minimal README.md
```

```markdown
# Project Name

Short description of what this does.

## Installation

\`\`\`bash
npm install
\`\`\`

## Usage

\`\`\`bash
npm start
\`\`\`
```

---

# PART 3: ADVANCED

## 3.1 Git Hooks

**What it is:** Scripts that Git automatically runs at specific points in the workflow (before a commit, after a commit, before a push, etc.). They live in `.git/hooks/`.

**Why use it:** Automate quality control — e.g., run linters/tests before allowing a commit, enforce commit message formats, prevent pushing directly to `main`, send notifications.

**Example:**

```bash
# .git/hooks/pre-commit  (make it executable: chmod +x pre-commit)
#!/bin/sh
echo "Running tests before commit..."
npm test
if [ $? -ne 0 ]; then
  echo "Tests failed — commit aborted."
  exit 1
fi
```

```bash
# .git/hooks/commit-msg — enforce a commit message format like "JIRA-123: message"
#!/bin/sh
if ! grep -qE "^[A-Z]+-[0-9]+: " "$1"; then
  echo "Commit message must start with TICKET-ID: e.g. 'PROJ-42: fix bug'"
  exit 1
fi
```

Common hook types: `pre-commit`, `commit-msg`, `pre-push`, `post-merge`, `pre-rebase`.
(Note: hooks are local-only by default; tools like **Husky** are used to share hooks across a team via the repo itself.)

---

## 3.2 Interactive Rebase

**What it is:** `git rebase -i` lets you edit, reorder, squash, split, or delete commits before they're replayed — a powerful history-editing tool.

**Why use it:** Clean up messy commit history before merging/pushing — e.g., squash 10 "WIP" commits into 1 meaningful commit, fix a typo in an old commit message, or reorder commits logically.

**Example:**

```bash
git rebase -i HEAD~5
```

This opens an editor showing the last 5 commits:

```
pick a1b2c3d Add login form
pick e4f5g6h Fix typo
pick h7i8j9k WIP
pick k1l2m3n Add validation
pick n4o5p6q Fix validation bug
```

You change `pick` to other commands:

```
pick   a1b2c3d Add login form
squash e4f5g6h Fix typo         # merge into commit above
drop   h7i8j9k WIP              # delete this commit entirely
pick   k1l2m3n Add validation
fixup  n4o5p6q Fix validation bug   # like squash, but discards this commit's message
```

Save and exit → Git replays commits according to your instructions.

---

## 3.3 Squash Commits

**What it is:** Combining multiple commits into a single commit (a specific use of interactive rebase, or done automatically via "squash merge" on GitHub/GitLab PRs).

**Why use it:** Keeps `main`'s history clean — one commit per feature/PR instead of dozens of "fix," "oops," "wip" commits.

**Example (interactive rebase method):**

```bash
git rebase -i HEAD~3
# change 'pick' to 'squash' (or 's') for the commits you want merged into the one above
```

**Example (GitHub PR squash merge):** When merging a pull request, choosing "Squash and merge" combines all commits in that PR into a single commit on `main` automatically.

---

## 3.4 Resolving Merge Conflicts

**What it is:** When Git can't automatically combine changes (e.g., two branches edited the same line differently), it pauses and marks the conflicting sections for you to resolve manually.

**Why it happens:** Two people (or branches) changed the same part of a file in different ways, and Git doesn't know which version is "correct."

**Example:**

```bash
git merge feature/login
# Auto-merging index.html
# CONFLICT (content): Merge conflict in index.html
```

The file will contain conflict markers:

```html
<<<<<<< HEAD
<h1>Welcome to my site</h1>
=======
<h1>Welcome to our platform</h1>
>>>>>>> feature/login
```

Resolve it by editing manually (keep one, keep both, or write something new):

```html
<h1>Welcome to our platform</h1>
```

Then finish:

```bash
git add index.html
git commit          # completes the merge
# (or, if mid-rebase: git rebase --continue)
```

**Helpful tools:**

```bash
git diff                      # see conflicts inline
git mergetool                 # open a visual merge tool (e.g., meld, vscode)
git merge --abort              # bail out completely and return to pre-merge state
```

---

## 3.5 Git Internals

**What it is:** Under the hood, Git is a content-addressable filesystem/key-value store built from four object types, all identified by SHA-1/SHA-256 hashes:

- **Blob** – stores raw file content (no filename, just data).
- **Tree** – represents a directory; lists blobs/trees with names and modes (like a folder structure).
- **Commit** – points to one tree (a full project snapshot) + parent commit(s) + author/message metadata.
- **Ref** – a human-readable pointer (branch/tag) to a commit hash. `HEAD` is a pointer to the current ref.

**Why understand this:** It demystifies "magic" Git commands, helps you recover from disasters, and explains _why_ Git is fast (it stores snapshots, not diffs, and identical content is deduplicated by hash).

**Example — see it yourself:**

```bash
# Find the hash of the current commit
git rev-parse HEAD

# Inspect any object by its hash
git cat-file -p <hash>        # -p = pretty-print

# See the type of an object
git cat-file -t <hash>

# See what's inside .git
ls .git/objects
ls .git/refs/heads             # branches
cat .git/HEAD                  # shows: ref: refs/heads/main

# Visualize the object graph
git log --graph --pretty=oneline --abbrev-commit --all
```

Example object chain:

```
commit  →  tree (root dir)  →  tree (src/)  →  blob (index.js content)
        →  parent commit(s)
```

---

## 3.6 Large Repository Management

**What it is:** Techniques for keeping Git usable and fast when a repo has huge history, huge binary files, or huge numbers of files.

**Why it matters:** Git wasn't originally built for massive binary assets (videos, datasets, design files) — cloning/fetching becomes painfully slow, and repo size balloons.

**Key techniques:**

**a) Shallow clones** — only fetch recent history, not the entire log:

```bash
git clone --depth 1 https://github.com/org/big-repo.git
```

**b) Sparse checkout** — only check out specific folders you need, not the whole tree:

```bash
git clone --no-checkout --filter=blob:none https://github.com/org/big-repo.git
cd big-repo
git sparse-checkout init --cone
git sparse-checkout set src/backend
```

**c) Partial clone (blobless/treeless)** — download file contents on-demand rather than all at once:

```bash
git clone --filter=blob:none https://github.com/org/big-repo.git
```

**d) Git LFS (Large File Storage)** — replaces large binary files (images, videos, datasets) with lightweight text pointers in the repo, storing actual content on a separate LFS server:

```bash
git lfs install
git lfs track "*.psd" "*.mp4"
git add .gitattributes
git add design.psd
git commit -m "Add design file via LFS"
git push
```

**e) History cleanup** — permanently remove large/unwanted files from all of history to shrink repo size:

```bash
# modern recommended tool: git-filter-repo
git filter-repo --path large-old-file.zip --invert-paths
```

**f) Maintenance/gc** — Git periodically repacks objects to save space and speed:

```bash
git gc --aggressive
git count-objects -v      # check repo size/object count
```

**g) Monorepo strategies** — for very large multi-team codebases: sparse-checkout + partial clone + tools like Google's `repo` tool or Meta's Sapling are often combined with Git itself.

---

# Quick Reference Cheat Sheet

| Goal                              | Command                                   |
| --------------------------------- | ----------------------------------------- |
| Start tracking a folder           | `git init`                                |
| Copy an existing repo             | `git clone <url>`                         |
| See what changed                  | `git status` / `git diff`                 |
| Save changes to a snapshot        | `git add .` → `git commit -m "msg"`       |
| View history                      | `git log --oneline --graph`               |
| Create/switch branch              | `git switch -c branch-name`               |
| Combine branches (keep history)   | `git merge branch-name`                   |
| Combine branches (linear history) | `git rebase branch-name`                  |
| Grab one commit from elsewhere    | `git cherry-pick <hash>`                  |
| Shelve unfinished work            | `git stash` / `git stash pop`             |
| Undo commit, keep changes         | `git reset --soft HEAD~1`                 |
| Undo commit, discard changes      | `git reset --hard HEAD~1`                 |
| Undo a pushed commit safely       | `git revert <hash>`                       |
| Mark a release                    | `git tag -a v1.0 -m "msg"`                |
| Clean up commit history           | `git rebase -i HEAD~n`                    |
| Automate checks                   | Git Hooks (`.git/hooks/`)                 |
| Handle huge repos                 | shallow clone / sparse-checkout / Git LFS |
| Sync with remote (safe peek)      | `git fetch`                               |
| Sync with remote (peek + merge)   | `git pull`                                |
| Upload your commits               | `git push`                                |
| Recover "lost" commits            | `git reflog` → `git reset --hard <hash>`  |
| Shortcut a long command           | `git config --global alias.xx "..."`      |
| Contribute to someone else's repo | Fork → clone → branch → PR                |
| Ignore files permanently          | `.gitignore`                              |
| Undo unstaged file changes        | `git restore <file>`                      |
| Unstage a file                    | `git restore --staged <file>`             |

---

**Suggested learning path:** Beginner commands daily for 1–2 weeks → practice branching/merging on a throwaway repo → deliberately create a merge conflict and resolve it → try interactive rebase on a test branch → read about Git internals once the workflow feels natural.

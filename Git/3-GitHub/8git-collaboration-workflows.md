# Git Collaboration Workflows

A guide to how teams collaborate using Git — from the simplest (and most problematic) setup to feature branches, pull requests, forking, and branch protection.

---

## 1. The Problems With Working on a Single Branch

### Centralized Workflow (a.k.a. everyone works on `main`/`master`)

This is the most basic collaboration model possible: everyone works directly on a single branch (`main`, `master`, or any other one branch). It's straightforward and can work for tiny teams, but it has serious shortcomings as a team grows.

**Problems with this approach:**

- **Constant merge conflicts** — Lots of time is spent resolving conflicts and merging code, especially as team size scales up.
- **No isolation for experimentation** — No one can work on anything without disturbing the main codebase. How do you try adding something radically different? How do you experiment safely?
- **Broken code gets shared** — The only way to collaborate on a feature together with a teammate is to push incomplete code to `main`. Now every other teammate has broken code too.

---

## 2. Feature Branch Workflow

Rather than working directly on `main`/`master`, all new development should be done on **separate branches**.

**Core principles:**

- Treat the `main`/`master` branch as the official, stable project history.
- Multiple teammates can collaborate on a single feature and share code back and forth without polluting `main`.
- `main`/`master` never contains broken code.

### Demo: Creating and Working on Feature Branches

```bash
# Create and switch to a new branch called "navbar"
git switch -c navbar

# Stage and commit your work
git add .
git commit -m "add navbar"

# Push the branch up to the remote
git push origin navbar
```

```bash
# See your local branches
git branch

# Make sure main is up to date
git pull origin main

# Create another feature branch
git switch -c pricing-table

git add .
git commit -m "add pricing table"
git push origin pricing-table
```

```bash
# View remote branches
git branch -r
```

> **Corrected commands** (common typos from the raw notes):
>
> - `git push origign navbar` → `git push origin navbar`
> - `git push orogin priceing-table` → `git push origin pricing-table`
> - `git branch -r pricing-tabel` → `git branch -r` (lists remote branches; there's no branch name argument needed)

---

## Merging in Feature Branches

At some point, work on feature branches needs to be merged back into `main`. There are a few options for how to do this:

1. **Merge at will, with no discussion** — just merge whenever you want. ⚠️ Risky for teams; no visibility or review.
2. **Send an email/chat message** to the team to discuss whether the changes should be merged.
3. **Pull Requests** — the standard, structured approach (covered below).

### Merging a Feature Branch Manually

```bash
ls
git status

# Switch to main and make sure it's current
git switch main
git pull origin main

# Merge the feature branch into main
git merge pricing-table

git status
git push origin main
```

> **Corrected commands:**
>
> - `git switc h main` → `git switch main`
> - `git pull orign main` → `git pull origin main`

---

## 3. Pull Requests

Pull requests (PRs) are a feature built into platforms like **GitHub**, **GitLab**, and **Bitbucket** — they are **not native to Git itself**.

**What pull requests do:**

- Alert team members to new work that needs to be reviewed.
- Provide a mechanism to **approve or reject** the work on a given branch.
- Facilitate **discussion and feedback** on the specific commits in the branch.

Essentially, a PR says: _"I have this new stuff I want to merge into `main` — what do you all think about it?"_

### The Pull Request Workflow

1. Do some work locally on a feature branch.
2. Push the feature branch up to GitHub (or your Git host).
3. Open a pull request using the feature branch just pushed up.
4. Start a discussion on the PR (this part depends on your team's structure).
5. Wait for the PR to be approved and merged.

### Making Your First Pull Request

- Push your feature branch to the remote repository.
- On GitHub/GitLab/Bitbucket, open a "New Pull Request," selecting your feature branch as the source and `main` as the target.
- Add a clear title and description explaining **what** changed and **why**.
- Request reviewers, and respond to any feedback or requested changes with additional commits.
- Once approved, the PR can be merged (via the platform's merge button, or manually — see below).

### Merging Pull Requests With Conflicts

If your feature branch has fallen behind `main` and conflicts appear, resolve them **on the feature branch first**:

```bash
# Update your local copy of all remote branches
git fetch origin

# Switch to your feature branch
git switch my-new-feature

# Merge main into your feature branch
git merge main

# Fix conflicts in your editor, then:
git add .
git commit -m "resolve merge conflicts with main"
git push origin my-new-feature
```

Once your feature branch merges cleanly, merge it into `main`:

```bash
git switch main
git merge my-new-feature
git push origin main
```

> **Corrected commands:**
>
> - `git mrege master` → `git merge main`
> - `git switch aster` → `git switch main`
> - `git merge m y0new-feature` → `git merge my-new-feature`
> - `git push origin master` → `git push origin main` (assuming your default branch is `main`)

---

## 4. Introducing Forking

### What Is Forking?

GitHub allows you to create a **personal copy of someone else's repository** — this copy is called a **fork** of the original. When you fork a repo, you're essentially asking GitHub: _"Make me my own copy of this repo, please."_

Just like pull requests, **forking is not a Git feature** — it's implemented by GitHub (and similarly by GitLab, Bitbucket, etc.), not by Git itself.

A fork is different from a branch:

- A **branch** lives inside the _same_ repository, and anyone with write access can push to it directly.
- A **fork** is a completely separate, independent copy of the _entire repository_, owned by you, under your own account — you can freely experiment without needing write access to the original ("upstream") repo.

**Why forking is used:**

- Common for **open-source contributions**, where external contributors don't have push access to the original repo.
- Lets contributors propose changes via a pull request **from their fork** back to the original repository.
- Keeps the original repository safe from unreviewed or experimental changes.

---

## 5. Fork and Clone Workflow

### Fork and Clone: Another Workflow

The fork and clone workflow is **different from anything covered so far**. Instead of just one centralized GitHub repository that everyone shares, **every developer has their own GitHub repository** in addition to the main ("upstream") repo.

Developers make changes and push to **their own fork** first, before opening a pull request against the original repository.

This workflow is extremely common on **large open-source projects**, where there may be thousands of contributors but only a handful of maintainers who have direct write access to the main repo. Forking lets anyone contribute without needing to be trusted with push access up front.

### Explaining the Forking Demonstration

Here's what a typical forking demonstration looks like, step by step:

1. **Find the repository you want to contribute to** on GitHub (the "upstream" repo) — you have read access but not write access to it.
2. **Click the "Fork" button** in the top-right of the repo page. GitHub creates a full copy of the repository under your own account, e.g. `github.com/your-username/project` — this is now _your_ repository, and you have full write access to it.
3. **Clone your fork** (not the original) to your local machine, so you have a working copy on your computer.
4. **Make your changes locally** — typically on a new feature branch inside your fork.
5. **Push your changes to your fork** (`origin`), _not_ to the original repository — you don't have permission to push there directly.
6. **Open a pull request** from your fork's branch back to the original repository's `main` branch. The maintainers review it, discuss it, and decide whether to merge it in.
7. **The original repo stays safe** the whole time — nothing you do in your fork can affect it until a maintainer approves and merges your pull request.

In short: **fork = your own copy of the whole repo on GitHub; clone = downloading that copy to your machine to work on it.**

### Fork and Clone Workflow — Commands

1. **Fork** the repository on GitHub — this creates a copy under your own account.
2. **Clone** your fork to your local machine:
   ```bash
   git clone https://github.com/your-username/project.git
   ```
3. **Add the original repository as a remote** (commonly called `upstream`), so you can pull in updates:
   ```bash
   git remote add upstream https://github.com/original-owner/project.git
   ```
4. **Create a feature branch** for your changes:
   ```bash
   git switch -c my-fix
   ```
5. **Make changes, commit, and push to your fork:**
   ```bash
   git add .
   git commit -m "fix typo in README"
   git push origin my-fix
   ```
6. **Open a pull request** from your fork's branch to the original repository's `main` branch.
7. **Keep your fork in sync** with the original repo periodically:
   ```bash
   git fetch upstream
   git switch main
   git merge upstream/main
   git push origin main
   ```

---

## 6. Branch Protection Rules

Branch protection rules let you **enforce quality and review standards** on important branches (typically `main`) so nobody can bypass the process, even accidentally.

### Configuring Branch Protection (GitHub example)

1. Go to your repository's **Settings**.
2. Click **Branches** in the sidebar.
3. Under "Branch protection rules," click **Add rule**.
4. In **Branch name pattern**, enter the branch to protect (e.g., `main`).
5. Enable **"Require a pull request before merging"**.
6. Enable **"Require approvals"** and set the number of required reviewers (e.g., 1).
7. Save the rule.

With this in place, changes to `main` can only happen through an approved pull request — direct pushes are blocked.

### Other Common Branch Protection Options (Extra)

- **Require status checks to pass before merging** (e.g., CI tests, linting, build success).
- **Require conversation resolution** before merging — all PR comments must be marked resolved.
- **Require signed commits** for extra security/authenticity verification.
- **Include administrators** — apply the rules even to repo admins/owners, not just regular contributors.
- **Restrict who can push to matching branches** — limit direct push access to specific people or teams.
- **Prevent force pushes and branch deletion** on protected branches, preserving history integrity.

---

## 7. Cleaning Up History With Interactive Rebase

### What Really Matters in This Section

- **Rewording commits** — changing a commit message without changing its content.
- **Fixup / squashing commits** — combining multiple commits into one.
- **Dropping commits** — removing a commit entirely from history.

### Introducing Interactive Rebase — Rewriting History

Sometimes we want to **rewrite, delete, rename, or even reorder commits** before sharing them with others. We can do this using `git rebase` in its **interactive** mode.

> ⚠️ Interactive rebase rewrites commit history. As a rule of thumb, only rebase commits that are **still local and haven't been pushed/shared** with others yet (or that you've explicitly coordinated with your team to rewrite).

### Running Interactive Rebase

Running `git rebase` with the `-i` flag enters **interactive mode**, which lets you:

- Edit commits
- Add/amend files within a commit
- Drop commits
- Reword commit messages
- Reorder commits
- Squash/fixup commits together

You need to specify **how far back** you want to rewrite history to. This is done by pointing to a commit reference — everything _after_ that commit becomes editable.

Also note: you are **not** rebasing onto another branch. Instead, you're rebasing a series of commits **onto the same `HEAD` they are currently based on** — you're just reordering/editing the commits that already exist, not moving them onto a different base.

```bash
# Rewrite the last 4 commits
git rebase -i HEAD~4
```

This opens an editor listing the last 4 commits, oldest first, each prefixed with the word `pick`:

```
pick a1b2c3d Add navbar
pick e4f5g6h Fix navbar spacing
pick h7i8j9k WIP: pricing table
pick k1l2m3n Fix typo in pricing table
```

### The Interactive Rebase Commands

You edit the word in front of each commit to tell Git what to do with it:

| Command  | Short | What it does                                                                                                              |
| -------- | ----- | ------------------------------------------------------------------------------------------------------------------------- |
| `pick`   | `p`   | Keep the commit as-is                                                                                                     |
| `reword` | `r`   | Keep the commit's changes, but edit its commit message                                                                    |
| `edit`   | `e`   | Pause at this commit so you can amend it (change file content)                                                            |
| `squash` | `s`   | Combine this commit into the previous one, **and** merge their commit messages (prompts you to edit the combined message) |
| `fixup`  | `f`   | Same as `squash`, but **discards** this commit's message entirely, keeping only the previous commit's message             |
| `drop`   | `d`   | Delete the commit entirely                                                                                                |

### Rewording a Commit

Change `pick` to `reword` (or `r`) next to the commit you want to rename:

```
reword a1b2c3d Add navbar
pick e4f5g6h Fix navbar spacing
pick h7i8j9k WIP: pricing table
pick k1l2m3n Fix typo in pricing table
```

Save and close — Git will pause and open your editor again just for that commit, letting you type a new commit message.

### Fixing Up / Squashing Commits

If you have messy "WIP" or "fix typo" commits that really belong to a previous commit, combine them using `squash` or `fixup`:

```
pick a1b2c3d Add navbar
pick e4f5g6h Fix navbar spacing
pick h7i8j9k WIP: pricing table
fixup k1l2m3n Fix typo in pricing table
```

Here, `k1l2m3n` ("Fix typo in pricing table") gets folded into `h7i8j9k` ("WIP: pricing table") — and because it's `fixup` (not `squash`), its own commit message is discarded, leaving one clean commit.

### Dropping Commits

To remove a commit entirely, change `pick` to `drop` (or simply delete that line from the list):

```
pick a1b2c3d Add navbar
pick e4f5g6h Fix navbar spacing
drop h7i8j9k WIP: pricing table
pick k1l2m3n Fix typo in pricing table
```

This permanently removes that commit's changes from the branch's history.

### Reordering Commits

Since the rebase list is just a text file, you can also **reorder commits** simply by changing the order of the lines — Git will replay them in the new order.

### After Editing

Once you save and close the interactive rebase file:

- Git replays your commits in order, applying each instruction.
- If you used `edit`, Git will pause at that commit — make your changes, then run:
  ```bash
  git add .
  git rebase --continue
  ```
- If conflicts occur during replay, resolve them, then run:
  ```bash
  git add .
  git rebase --continue
  ```
- You can abort at any point and return to the pre-rebase state with:
  ```bash
  git rebase --abort
  ```

### Pushing Rewritten History

Because interactive rebase **rewrites commit hashes**, a normal `git push` will be rejected if you've already pushed the old commits. You'll need a **force push**:

```bash
git push --force-with-lease origin your-branch-name
```

> `--force-with-lease` is safer than a plain `--force` — it fails if someone else has pushed new commits to the branch since you last fetched, preventing you from accidentally overwriting their work.

---

## 8. Git Tags — Marking Important Moments in History

### Understanding Git Tags

Tags are **pointers that refer to a particular point in Git history**. We can mark a particular moment in time with a tag — tags are most often used to **mark version releases** in a project.

Think of a tag like a branch reference that **does not change**. Once a tag is created, it always refers to the same commit — it's just a fixed label for a commit, unlike a branch pointer which moves forward as new commits are added.

### The Two Types of Tags

There are two types of Git tags: **lightweight** and **annotated**.

- **Lightweight tags** — just a name/label that points to a particular commit. No extra metadata.
- **Annotated tags** — store extra metadata, including the tagger's name and email, the date, and a tagging message. (Similar to a commit — recommended for anything you plan to share, like release tags.)

---

### Semantic Versioning

The **Semantic Versioning** (SemVer) spec outlines a standardized versioning system for software releases. It provides a consistent way for developers to give meaning to their software releases.

A version number consists of **three numbers separated by periods**:

```
2 . 4 . 1
│   │   │
│   │   └── Patch release
│   └────── Minor release
└────────── Major release
```

**Initial release**

Typically, the first release of a project is `1.0.0`.

**Patch release — `1.0.1`**

Patch releases normally do not contain new features or significant changes. They typically signify bug fixes and other changes that do not impact how the code is used.

**Minor release — `1.1.0`**

Minor releases signify that new features or functionality have been added, but the project is still **backwards compatible** — no breaking changes. The new functionality is optional and should not force users to rewrite their own code.

**Major release — `2.0.0`**

Major releases signify significant changes that are **no longer backwards compatible**. Features may be removed or changed substantially.

---

### Viewing Tags

`git tag` will print a list of all the tags in the current repository:

```bash
git tag
```

We can search for tags that match a particular pattern using `git tag -l` and passing in a wildcard pattern. For example, this prints a list of all tags that include "beta" in their name:

```bash
git tag -l '*beta*'
```

Other examples:

```bash
git tag -l          # list all tags
git tag -l "*17*"   # list tags matching a pattern, e.g. containing "17"
```

---

### Comparing Tags With `git diff`

We can check out a specific tag and use `git diff` to compare it against another tag, branch, or commit:

```bash
git checkout 15.3.1
git tag
git diff 15.3.1 16.0.0
```

This shows exactly what changed between the two tagged points in history — useful for release notes or auditing what a version bump actually contains.

---

### Creating Lightweight Tags

To create a lightweight tag, use `git tag <tagname>`. By default, Git will create the tag referring to the commit that `HEAD` is currently referencing.

```bash
git tag <tagname>
```

---

### Creating Annotated Tags

Use `git tag -a` to create a new annotated tag. Git will then open your default text editor and prompt you for additional information (similar to `git commit`).

```bash
git tag -a <tagname>
```

We can also use the `-m` option to pass a message directly and skip opening the text editor:

```bash
git tag -a <tagname> -m "Release version 2.0.0"
```

---

### Tagging Previous Commits

So far we've seen how to tag the commit that `HEAD` references. We can also tag an **older commit** by providing its commit hash:

```bash
git tag -a <tagname> <commit-hash>

# Also works for lightweight tags:
git tag <tagname> <commit-hash>
```

---

### Moving / Forcing Tags

Git will refuse (and yell at you) if you try to reuse a tag name that already refers to a commit — this prevents you from accidentally moving a tag and rewriting release history. If you use the `-f` (force) option, you can force the tag to move/be reassigned to a new commit:

```bash
git tag -f <tagname> <new-commit-hash>
```

> ⚠️ Use this carefully — if the tag has already been pushed and others have pulled it, moving it locally can cause confusion, since remote and local tags will now disagree until you force-push the change too.

---

### Deleting Tags

To delete a tag locally, use `-d`:

```bash
git tag -d <tagname>
```

To delete a tag that has already been pushed to a remote, you need to delete it there separately — deleting it locally does **not** remove it from the remote:

```bash
git push origin --delete <tagname>

# Alternative syntax:
git push origin :refs/tags/<tagname>
```

---

### Pushing Tags

By default, `git push` **does not** transfer tags to remote servers — tags are not pushed automatically along with commits.

To push a single tag:

```bash
git push origin <tagname>
```

If you have a lot of tags you want to push up at once, use the `--tags` option with `git push`. This transfers **all** of your local tags to the remote server that aren't already there:

```bash
git push --tags
```

---

## Extra Points (Beyond the Original Notes)

- **Naming conventions:** Many teams prefix feature branches for clarity, e.g. `feature/navbar`, `bugfix/login-error`, `hotfix/critical-patch`.
- **Merge strategies:** When merging PRs, GitHub/GitLab typically offer three options:
  - **Merge commit** — preserves full history with a merge commit.
  - **Squash and merge** — combines all feature branch commits into a single commit on `main` for a cleaner history.
  - **Rebase and merge** — replays feature commits on top of `main` without a merge commit.
- **Draft pull requests:** Many platforms let you open a PR as a "draft" to signal work-in-progress and get early feedback before it's ready for formal review.
- **Continuous Integration (CI):** It's common to pair pull requests with automated CI pipelines that run tests/builds on every push, catching issues before human review even starts.
- **Code owners:** Some repos use a `CODEOWNERS` file to automatically request reviews from the right people/teams based on which files changed.
- **Stale branch cleanup:** After a feature branch is merged, delete it (locally and remotely) to keep the repository tidy:
  ```bash
  git branch -d navbar          # delete local branch
  git push origin --delete navbar  # delete remote branch
  ```
- **Rebasing vs. merging:** As an alternative to `git merge`, `git rebase` replays your commits on top of the latest `main`, producing a linear history — but should generally be avoided on shared/public branches since it rewrites commit history.
- **Checking out a tag = detached HEAD:** When you `git checkout <tagname>`, you're not on a branch anymore — you're in a "detached HEAD" state. If you want to make new commits from that point, create a branch first: `git switch -c new-branch-name <tagname>`.
- **Tags vs. releases:** On GitHub, a "Release" is a platform feature built on top of a Git tag — it lets you attach release notes, binaries, and changelogs to a tag. Like forking and pull requests, Releases are a GitHub feature, not a native Git concept.
- **Pre-release tags:** SemVer also supports pre-release identifiers for versions still in testing, e.g. `2.0.0-beta.1` or `2.0.0-rc.1` (release candidate), which sort before the final `2.0.0`.

---

_Note: This guide assumes `main` as the default branch name, which is the modern GitHub/GitLab standard (replacing the older `master` convention)._

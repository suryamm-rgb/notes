# Git Fetch and Pull — Study Notes

## 1. Remote Tracking Branches

When you **clone** a repository, you get all the data and Git history for the
project *at that moment in time*. But that doesn't mean everything is
automatically loaded into your local workspace.

**Example scenario:**
> The GitHub repo has a branch called `puppies`, but when you run `git branch`
> you don't see it on your machine — only `master` shows up. What's going on?

That's because `git branch` (with no flags) only shows your **local**
branches. To see the branches Git knows about on the **remote**, use:

```bash
git branch -r
```

This will show something like:

```
origin/master
origin/puppies
```

These are called **remote-tracking branches** — read-only references that
show you the state of branches on the remote server, as of your last fetch.

### Checking out a remote-tracking branch

If you want to actually work on `puppies` locally, check it out:

```bash
git checkout puppies
# or, explicitly:
git checkout -b puppies origin/puppies
```

Git automatically creates a local branch that tracks `origin/puppies`.

---

## 2. Git Fetch

**Fetching** allows you to download changes from a remote repository, but
those changes are **not automatically merged** into your working files.

Think of it as:
> "Please go get the latest information from GitHub, but don't touch my
> working directory."

### Basic usage

```bash
git fetch <remote>
```

Example — fetch everything from `origin`:

```bash
git fetch origin
```

Fetch a **specific branch** from a remote:

```bash
git fetch <remote> <branch>
```

Example:

```bash
git fetch origin puppies
```

### What `git fetch` does

- Downloads new commits/branches/tags from the remote repository
- Updates your **remote-tracking branches** (e.g. `origin/master`)
- Does **NOT** merge anything into your current `HEAD` branch
- Safe to run at any time — it never changes your working files

### Example workflow

```bash
git fetch origin
git log origin/master   # see what's new on the remote, before merging
```

---

## 3. Git Pull

**Pulling** also retrieves changes from a remote repository, but unlike
fetch, `git pull` **updates your current branch** with whatever changes it
downloads.

Think of it as:
> "Download data from GitHub and immediately update my local branch with
> those changes."

### The formula

```
git pull = git fetch + git merge
```

Specifically, `git pull`:
1. Updates the remote-tracking branch with the latest changes from the remote
2. Merges those changes into your **current local branch**

### Basic usage

```bash
git pull
git pull <remote> <branch>
```

Example:

```bash
git pull origin master
```

This fetches the latest info from `origin/master` and **merges** it into
your current branch.

⚠️ Because pull performs a merge automatically, it **can result in merge
conflicts** if your local changes and the remote changes touch the same
lines.

---

## 4. Fetch vs. Pull — Quick Comparison

| | `git fetch` | `git pull` |
|---|---|---|
| Gets changes from remote | ✅ | ✅ |
| Updates remote-tracking branches | ✅ | ✅ |
| Merges changes into current branch | ❌ | ✅ |
| Can cause merge conflicts | ❌ (safe) | ✅ possible |
| Safe with uncommitted changes | ✅ | ⚠️ not recommended |
| Good for | Reviewing changes before merging | Quickly syncing your branch |

**Rule of thumb:**
- Use `git fetch` when you want to **see** what's changed without touching
  your working directory.
- Use `git pull` when you're ready to **bring those changes into** your
  current branch — and ideally when you have no uncommitted work in
  progress.

---

## 5. Putting It All Together — Example

```bash
# 1. See what remote branches exist
git branch -r
# origin/master
# origin/puppies

# 2. Fetch the latest data (doesn't touch your files)
git fetch origin

# 3. Inspect what's new before merging
git log HEAD..origin/master

# 4. If it looks good, merge it in manually...
git merge origin/master

# ...or just do it all in one step next time:
git pull origin master
```

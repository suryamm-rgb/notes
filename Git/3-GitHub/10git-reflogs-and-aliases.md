# The Power of Reflogs: Retrieving Lost Work (+ Custom Git Aliases)

How Git secretly tracks everything you do locally — and how to use that safety net to recover "lost" commits. Plus, how to speed up your workflow with custom Git aliases.

---

## What's Covered

- Exploring the reflog
- The `git reflog` command
- Rescuing lost commits with reflog
- Undoing a rebase with reflog
- Time-based reflog qualifiers
- Writing custom Git aliases

---

## Introduction to Reflogs

Git keeps a record of when the **tips of branches and other references were updated** in the repo. This record is called the **reflog** (reference log).

We can view and use these reference logs with the `git reflog` command.

Think of the reflog as Git's personal diary of "everywhere `HEAD` and your branches have pointed" — even commits that get orphaned by a reset, rebase, or branch deletion are still remembered here for a while, which makes reflog an incredibly powerful safety net.

### The Limitations

- **Reflogs are local only.** Git only keeps reflogs of *your own* local activity — they are **not shared with collaborators** and are not pushed to a remote. If you clone a fresh copy of a repo, its reflog starts empty.
- **Reflogs expire.** Git cleans out old entries automatically — by default, after around **90 days** — though this is configurable.

---

## The `git reflog show` Command

The `git reflog` command accepts several subcommands: `show`, `expire`, `delete`, and `exists`. `show` is the only commonly used one, and it's also the **default subcommand** (so `git reflog` alone behaves like `git reflog show`).

`git reflog show` displays the log of a **specific reference** — it defaults to `HEAD` if you don't specify one.

For example, to view the logs for the tip of the `main` branch specifically:

```bash
git reflog show main
```

To view the log for `HEAD` (the default, and most common use case):

```bash
git reflog show HEAD
# or simply:
git reflog
```

Each line in the output shows a reflog entry: a commit hash, which `HEAD` position that was, and a description of the action that moved the pointer there (e.g. `commit`, `checkout`, `rebase`, `reset`, `merge`, etc.).

---

## Reflog References

We can access a specific reflog entry using the syntax:

```
name@{qualifier}
```

This lets us reference a specific historical pointer position — for example, "where `HEAD` was 2 moves ago" — and we can pass this reference to other commands, including `checkout`, `reset`, and `merge`.

Examples:

```bash
git checkout HEAD@{2}     # go to where HEAD was 2 reflog entries ago
git reset --hard HEAD@{1} # reset back to where HEAD was 1 entry ago
```

---

## Time-Based Reflog Qualifiers

Every entry in the reflog has a **timestamp** associated with it. We can filter/reference reflog entries by time or date using time-based qualifiers, such as:

```
1.day.ago
3.minutes.ago
yesterday
```

Example — check where the `master` branch pointed one week ago:

```bash
git reflog master@{one.week.ago}
```

You can use this same syntax anywhere a commit reference is expected:

```bash
git checkout master@{yesterday}
git diff master@{2.days.ago} master
```

---

## Rescuing Lost Commits With Reflog

We can sometimes use reflog entries to recover commits that **seem lost** and no longer appear in `git log` — for example, after:

- An accidental `git reset --hard` that dropped commits
- Deleting a branch before merging it
- Amending a commit and losing the original version
- A rebase that went wrong (see below)

**General recovery process:**

```bash
# 1. Find the "lost" commit in the reflog
git reflog

# 2. Once you spot the commit hash (or HEAD@{n}) you want back, either:

# Option A: Check it out directly to inspect it (detached HEAD)
git checkout <commit-hash>

# Option B: Create a new branch pointing at it, to fully recover it
git branch recovered-work <commit-hash>

# Option C: Reset your current branch back to it
git reset --hard <commit-hash>
```

> 💡 As long as the commit still shows up *somewhere* in the reflog, it hasn't actually been deleted from Git's object database yet — it's just no longer reachable from any branch. Reflog gives you a way back to it before Git's garbage collector eventually cleans it up.

---

## Undoing a Rebase With Reflog

Rebases rewrite commit history, which can be scary — but reflog makes rebases much safer to experiment with, because the **pre-rebase state is preserved** in the reflog.

If a rebase goes wrong (bad conflict resolution, wrong commits squashed, etc.), you can find the commit your branch pointed to **right before the rebase started** and reset back to it:

```bash
git reflog
# Look for an entry like: <hash> HEAD@{5}: rebase (start): checkout <target>

git reset --hard HEAD@{5}
```

This effectively "undoes" the entire rebase, restoring your branch to exactly where it was beforehand.

---

## Writing Custom Git Aliases

### The Global Git Config File

Git looks for the global config file at either `~/.gitconfig` or `~/.config/git/config`. Any configuration variable changed in this file is applied **across all Git repositories** on your machine.

We can edit this file directly, or set configuration values from the command line — whichever is preferred.

```bash
git config --global user.name
git config --global user.email

cat ~/.gitconfig
```

---

### Writing Your First Git Alias

We can set up Git aliases to make our Git workflow simpler and faster. For example:

- Define `git ci` as a shortcut instead of typing `git commit`.
- Define a custom `git lg` command that prints a nicely formatted commit log.

Aliases can be added directly inside `~/.gitconfig` under an `[alias]` section:

```ini
[alias]
    s = status
    l = log
    ci = commit
```

Now `git s` works exactly like `git status`, `git l` like `git log`, and `git ci` like `git commit`.

---

### Setting Aliases From the Command Line

You don't have to edit the config file by hand — you can set aliases directly via `git config`:

```bash
git config --global alias.showmebranches branch
git showmebranches
```

This automatically adds the equivalent entry to your `~/.gitconfig` file's `[alias]` section.

---

### Aliases With Arguments

Aliases can also include flags/options baked right in, so a shortcut runs a more complex command:

```bash
git config --global alias.lg "log --oneline --graph --decorate --all"
```

Now `git lg` prints a compact, graph-based view of the full commit history:

```bash
git lg
```

You can even build aliases that shell out to run non-Git commands, by prefixing with `!`:

```bash
git config --global alias.today "!git log --since=midnight --author='$(git config user.name)' --oneline"
```

---

### Exploring Existing Useful Aliases Online

Many developers share their favorite Git alias collections publicly (e.g. on GitHub Gists, dotfiles repositories, and blog posts). Some popular examples worth looking into:

- `git undo` → undo the last commit but keep the changes staged
- `git amend` → shortcut for `commit --amend --no-edit`
- `git last` → show the last commit's details
- `git unstage` → shortcut for `reset HEAD --`

Searching for "git aliases dotfiles" or browsing popular dotfiles repos on GitHub is a great way to discover time-saving aliases other developers rely on daily.

---

## Extra Points (Beyond the Original Notes)

- **Reflog isn't a full backup — it's temporary.** Once an unreachable commit's reflog entry expires (default ~90 days) *and* garbage collection runs (`git gc`), the commit is eligible for permanent deletion. Don't treat reflog as a long-term backup strategy — recover important work promptly.
- **Configuring reflog expiration:** You can change the default expiration windows with:
  ```bash
  git config gc.reflogExpire "90 days"
  git config gc.reflogExpireUnreachable "30 days"
  ```
- **Reflog vs. `git log`:** `git log` shows commit history reachable from the current branch/HEAD — it's about *what the project's history is*. `git reflog` shows *where your HEAD/branches have physically pointed over time* — it's about your local activity, regardless of whether those commits are still "in" the project history.
- **Finding dangling commits directly:** As an alternative/complement to reflog, `git fsck --lost-found` can locate unreachable ("dangling") commits and blobs in the object database.
- **Viewing all local reflogs at once:** `git reflog show --all` lists reflog entries across all references, not just `HEAD`.
- **Removing an alias:** To delete a previously set alias:
  ```bash
  git config --global --unset alias.showmebranches
  ```
- **Listing all your current aliases:**
  ```bash
  git config --global --get-regexp alias
  ```

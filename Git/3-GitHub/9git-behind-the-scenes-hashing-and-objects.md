# Git Behind the Scenes: Hashing and Objects

A look under the hood at how Git actually stores your project — the `.git` folder, hashing, and Git's internal object model.

---

## What's Covered

- The local config file
- The `refs` directory
- The `HEAD` file
- Hashing functions — the basics
- Git objects: blobs, trees, commits, and more

---

## Working With the Local Config File

### What's Inside `.git`?

Every Git repository has a hidden `.git` folder at its root. Inside, you'll find (among other things):

```
.git/
├── config
├── HEAD
├── index
├── objects/
└── refs/
```

There's more in there, but these are the "juicy" parts worth understanding.

### `config`

The `config` file is for configuration. We've seen how to configure **global** settings — like your name and email — across *all* Git repos on a machine (`git config --global ...`). But Git also lets you configure things on a **per-repository** basis, stored right here in `.git/config`. Anything set locally in this file overrides the global config for that repository only.

---

## The `refs` Directory

Inside `refs/`, you'll find a `heads` directory: `refs/heads/`.

- `refs/heads/` contains **one file per branch** in the repository.
- Each file is named after a branch, and its contents are simply the **commit hash** at the tip of that branch.
- For example, `refs/heads/master` contains the commit hash of the last commit on the `master` branch.

`refs` also contains a `refs/tags/` folder, which contains **one file for each tag** in the repo — same idea, but for tags instead of branches.

### Exploring It Yourself

```bash
git branch
ls .git/
ls .git/refs
ls .git/refs/heads
```

Try `cat .git/refs/heads/main` — you'll see a raw 40-character commit hash, exactly matching what `git log` shows as the latest commit on that branch. This is literally *all* a branch is under the hood: a name pointing at a commit hash.

---

## Inside the `HEAD` File

`HEAD` is just a plain text file that keeps track of **where `HEAD` currently points**.

- If it contains something like `ref: refs/heads/master`, that means `HEAD` is pointing to the `master` branch (i.e., you're "on" that branch, and `HEAD` will move automatically as new commits are made there).
- In a **detached HEAD** state, the `HEAD` file instead contains a **raw commit hash** directly, rather than a branch reference — meaning you're pointed at a specific commit, not "riding along" with any branch.

```bash
cat .git/HEAD
```

---

## The `objects` Folder

The `objects/` directory contains all of the repo's actual file data — this is where Git stores backups of file contents, the commits in a repo, and more. It's effectively Git's database.

These files are all **compressed and hashed** (not human-readable), so if you peek inside, they won't look like much — just binary-looking blobs named after hashes.

### The 4 Types of Git Objects

1. **Blob** — stores file contents.
2. **Tree** — stores directory structure (references to blobs and other trees).
3. **Commit** — stores a snapshot reference plus metadata (author, message, parent, etc.).
4. **Annotated Tag** — stores a reference to a commit plus metadata (tagger, date, message).

---

## The Git Database — A Key-Value Store

Git is, at its core, a **key-value data store**. We can insert any kind of content into a Git repository, and Git will hand us back a **unique key** we can later use to retrieve that exact content.

These keys are **SHA-1 checksums** — 40-character hexadecimal hashes computed from the content itself.

```bash
echo 'hello' | git hash-object --stdin
```

This returns a 40-character SHA-1 hash. Since the hash is derived purely from the *content*, the exact same content will **always** produce the exact same hash — no matter when, where, or by whom it's hashed. This is also why Git can detect duplicate content and store it only once.

---

## Blobs

**Blobs** ("Binary Large Objects") are the object type Git uses to store the **contents of files** in a repository.

Key point: blobs **don't include the filename**, or any other metadata about the file — no name, no permissions, no timestamp. They store **only the raw content** of the file. Two files with identical content (regardless of name or location) will hash to the exact same blob.

---

## Trees

**Trees** are Git objects used to store the **contents of a directory**.

- Each tree contains pointers that can refer to **blobs** (files) and to **other trees** (subdirectories) — this is how Git represents nested folder structures.
- Each entry in a tree includes:
  - The **SHA-1 hash** of the blob or tree it points to
  - The **mode** (e.g., file permissions/type, like a regular file vs. executable vs. directory)
  - The **type** (`blob` or `tree`)
  - The **file/directory name**

So a tree is essentially a snapshot listing of "here's what's in this folder, and here's the hash of each thing's content" — filenames live in the tree, not in the blob itself.

---

## Commits

**Commit** objects combine a **tree object** (the full snapshot of the project at that point) along with information about the **context** that led to the current tree.

Every commit stores:

- A reference to the **tree** representing the project snapshot
- A reference to the **parent commit(s)** (the commit(s) that came before it)
- The **author** (who wrote the change)
- The **committer** (who applied the change — can differ from the author, e.g. after a rebase)
- The **commit message**

Because each commit points to its parent, this chain of pointers is what forms the project's entire history — a commit is really just a snapshot pointer plus "what came before this and why."

---

## Putting It All Together

Here's how the pieces connect, from the ground up:

```
Commit
  ├── points to → Tree (root of the project snapshot)
  │                 ├── points to → Blob (file contents)
  │                 ├── points to → Blob (file contents)
  │                 └── points to → Tree (subdirectory)
  │                                   └── points to → Blob (file contents)
  └── points to → Parent Commit (previous commit, forming history)
```

- A **branch** (`refs/heads/<name>`) is just a movable pointer to a commit hash.
- **`HEAD`** points to either a branch (normal state) or directly to a commit hash (detached HEAD state).
- A **tag** (`refs/tags/<name>`) is a fixed pointer to a commit — either directly (lightweight) or via an annotated tag object with extra metadata.

Every single object — blob, tree, commit, or annotated tag — is identified and retrieved purely by its **SHA-1 hash**, making Git's entire object database content-addressable.

---

## Extra Points (Beyond the Original Notes)

- **Inspecting objects directly:** You can look inside any Git object using `git cat-file`:
  ```bash
  git cat-file -p <hash>   # pretty-print the object's contents
  git cat-file -t <hash>   # show the object's type (blob/tree/commit/tag)
  ```
- **Object storage location:** Objects are stored in `.git/objects/` using the first 2 characters of their hash as a subfolder name, and the remaining 38 characters as the filename — e.g. hash `a1b2c3...` is stored at `.git/objects/a1/b2c3...`. This keeps any single folder from holding too many files.
- **Compression, not encryption:** Objects in `.git/objects/` are **zlib-compressed**, not encrypted — anyone with access to the repo can decompress and read them. SHA-1 hashing provides content-addressing and integrity checking, not confidentiality.
- **The `index` file:** Sitting alongside `objects`, `refs`, `config`, and `HEAD`, the `index` file is Git's **staging area** — it tracks exactly what will go into your *next* commit when you run `git add`.
- **Why Git is fast at detecting changes:** Because objects are content-addressed by hash, Git can instantly tell if a file has changed just by comparing hashes — no need to diff full file contents to check for a match.
- **SHA-1 vs. newer hashing:** Git has historically used SHA-1, but due to known theoretical collision weaknesses, Git has been transitioning to support **SHA-256** as an optional alternative hashing algorithm for repositories that want stronger collision resistance.
- **Packfiles:** Over time, Git compresses many loose objects together into single **packfiles** (`.pack`) for efficiency — this happens automatically during operations like `git gc` (garbage collection) and when pushing/cloning.

# 1\. What is a Git Branch?

A **branch** is an independent line of development inside a Git repository.

Think of a branch as your own private workspace where you can make changes without affecting the main project.

Instead of everyone editing the same code, each developer creates their own branch.

Later, Git combines everyone's work together.

* * *

## Example

Suppose you're building an E-Commerce application.

Current project

    main
    
    Home Page
    Products
    Cart
    Login

Manager says:

> "Build a Wishlist feature."

Instead of editing the main branch:

    main

Create a new branch

    main
       │
       └──────── wishlist

All wishlist work happens here.

If something breaks...

    wishlist ❌
    
    main ✅ Safe

Nothing happens to the main branch.

* * *

# 2\. Why Do We Need Branches?

Branches let us:

*   Build new features
*   Fix bugs
*   Experiment
*   Work with multiple developers
*   Test ideas safely

Without branches

    Developer A
            \
             \
    main ------------
             /
    Developer B

Everyone edits the same files.

Problems:

*   Conflicts
*   Bugs
*   Broken production
*   Difficult teamwork

* * *

With branches

                    login-feature
                  /
    main ---------
                  \
                   payment-feature

Each developer works independently.

Later everything gets merged.

* * *

# 3\. Main (Master) Branch

Every Git repository starts with one default branch.

Historically its name was:

    master

Nowadays Git uses

    main

Most companies now use **main**.

Older repositories still use **master**.

They are exactly the same.

The branch name has no special power.

It's just another branch.

* * *

Example

    main
    
    Commit A
    Commit B
    Commit C

* * *

# 4\. What is HEAD?

One of the most important Git concepts.

**HEAD** is simply a pointer.

It points to the branch you're currently working on.

Example

    HEAD
     ↓
    
    main

If you switch branches

    git switch login

Now

    HEAD
     ↓
    
    login

HEAD always tells Git:

> "This is where I'm currently working."

* * *

Example

    main
    
    A --- B --- C
                 ↑
               HEAD

After switching

    login
    
    A --- B --- C
                 ↑
              login
                ↑
              HEAD

* * *

HEAD never points randomly.

Usually it points to

    HEAD
    ↓
    
    Current Branch
    ↓
    
    Latest Commit

* * *

# 5\. Viewing Branches

To list all local branches:

    git branch

Example

    * main
      login
      payment
      bugfix

The `*` indicates the current branch.

    HEAD
     ↓
    
    main

* * *

Show last commit on each branch:

    git branch -v

Example

    * main     9d67d2 Add homepage
      login    c132ab Login completed
      payment  a82f1d Payment gateway

* * *

# 6\. Creating Branches

Command

    git branch <branch-name>

Example

    git branch login

Important:

This **creates** the branch.

It **does NOT switch** to it.

Example

Current

    HEAD
    ↓
    
    main

Run

    git branch login

Result

    main
    login

Still

    HEAD
    ↓
    
    main

You're still on `main`.

* * *

# 7\. Switching Branches

Use

    git switch <branch-name>

Example

    git switch login

Output

    Switched to branch 'login'

Now

    HEAD
    ↓
    
    login

* * *

Verify

    git branch

    main
    * login

* * *

# 8\. `git switch` vs `git checkout`

Historically

    git checkout login

was used.

Today

    git switch login

is preferred.

### Why?

Because `checkout` does many different things.

It can

*   switch branches
*   restore files
*   checkout commits
*   detach HEAD

Too many responsibilities.

Git introduced

    git switch

just for changing branches.

* * *

Old way

    git checkout login

New way

    git switch login

Both work.

Modern Git recommends `switch`.

* * *

# 9\. Create and Switch in One Command

Instead of

    git branch login
    
    git switch login

Use

    git switch -c login

`-c` means

    create

Example

    git switch -c payment

Git will

*   create branch
*   switch to it

in one command.

* * *

# 10\. Switching with Uncommitted Changes

Suppose

    main

You edit

    app.js

But don't commit.

    Modified

Now run

    git switch login

Possible outcomes:

### If the changes don't conflict

Git switches successfully.

* * *

### If the changes conflict

Git blocks switching.

Example

    error:
    Your local changes would be overwritten.

Options:

Commit changes

    git add .
    
    git commit -m "Work in progress"

or

Stash changes

    git stash

Then switch

    git switch login

* * *

# 11\. Renaming Branches

Rename the current branch

    git branch -m new-name

Example

Current

    abc

Rename

    git branch -m feature-login

Now

    feature-login

* * *

Rename another branch

    git branch -m old-name new-name

Example

    git branch -m abc login

* * *

# 12\. Deleting Branches

Delete a fully merged branch

    git branch -d login

Git only deletes it if it has already been merged.

* * *

Example

    main
    login

Switch first

    git switch main

Delete

    git branch -d login

* * *

# 13\. Force Delete Branches

If branch isn't merged

Git refuses.

Example

    error:
    
    The branch is not fully merged.

Force delete

    git branch -D login

`-D`

means

    Force Delete

Use carefully.

* * *

# 14\. `git commit -a -m`

Example

    git commit -a -m "Add two moderate songs"

What happens?

The `-a` flag automatically stages **modified and deleted tracked files** before committing.

Equivalent to:

    git add <modified-files>
    git commit -m "Add two moderate songs"

### Important

`-a` **does not stage new (untracked) files**.

Example:

    touch new.txt
    git commit -a -m "message"

`new.txt` will **not** be committed.

You must first add it:

    git add new.txt
    git commit -m "Add new file"

Use `git commit -a -m` when you've only modified existing tracked files and want a quicker workflow.

* * *

# 15\. `git stash`

Sometimes you're halfway through a feature but need to switch branches.

Your work isn't ready to commit.

Use

    git stash

Git temporarily saves your uncommitted changes and restores a clean working directory.

Switch branches:

    git switch main

Later, return and restore the changes:

    git switch feature
    git stash pop

This reapplies the most recently stashed changes and removes them from the stash list.

* * *

# 16\. Branching Exercise (Harry Potter)

## Step 1 — Create a Project

    mkdir Patronus
    cd Patronus

* * *

## Step 2 — Initialize Git

    git init

* * *

## Step 3 — Create File

    touch patronus.txt

* * *

## Step 4 — Commit Empty File

    git add patronus.txt
    git commit -m "add empty patronus file"

* * *

## Step 5 — Create Branches

    git branch harry
    git branch snape

* * *

## Step 6 — Switch to `harry`

    git switch harry

* * *

## Step 7 — Add Harry's Patronus

Paste the provided ASCII art into `patronus.txt` with the heading:

    HARRY'S PATRONUS

* * *

## Step 8 — Commit

    git add patronus.txt
    git commit -m "add harry's stag patronus"

* * *

## Step 9 — Switch to `snape`

    git switch snape

* * *

## Step 10 — Add Snape's Patronus

Replace the file contents with the provided ASCII art and heading:

    SNAPE'S PATRONUS

* * *

## Step 11 — Commit

    git add patronus.txt
    git commit -m "add snape's doe patronus"

* * *

## Step 12 — Create `lily` from `snape`

Since you're currently on `snape`:

    git switch -c lily

This creates `lily` based on the latest commit in `snape`.

* * *

## Step 13 — You're Already on `lily`

Verify:

    git branch

You should see:

      harry
    * lily
      main
      snape

* * *

## Step 14 — Edit the File

Change only the heading from:

    SNAPE'S PATRONUS

to:

    LILY'S PATRONUS

Leave the doe ASCII art unchanged.

* * *

## Step 15 — Commit

    git add patronus.txt
    git commit -m "add lily's doe patronus"

* * *

## Step 16 — List All Branches

    git branch

Expected output:

      harry
    * lily
      main
      snape

* * *

## Step 17 — Bonus: Delete `snape`

You cannot delete the branch you're currently on, so switch away first:

    git switch main
    git branch -d snape

If Git refuses because `snape` hasn't been merged:

    git branch -D snape

* * *

# 17\. Final Branch Structure

                     harry
                    /
    main ----------
    
                     snape
                        \
                         lily

`harry` and `snape` were created from `main`, while `lily` was created from `snape`, inheriting all of `snape`'s commits.

* * *

# 18\. Common Interview Questions

### What is a Git branch?

A branch is an independent line of development that lets you work on changes without affecting other branches.

### What is `HEAD`?

`HEAD` is a pointer to the currently checked-out branch and its latest commit.

### Does `git branch` switch branches?

No. It only creates or lists branches. Use `git switch` (or `git checkout`) to change branches.

### Difference between `git switch` and `git checkout`?

*   `git switch` is dedicated to changing branches.
*   `git checkout` is older and can switch branches, restore files, and check out commits.

### Difference between `-d` and `-D`?

*   `git branch -d` safely deletes only merged branches.
*   `git branch -D` force-deletes a branch, even if it contains unmerged commits.

### When should I use `git stash`?

Use it when you need to temporarily save uncommitted changes so you can switch branches or pull updates without making a temporary commit.
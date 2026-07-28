# GitHub: The Basics

## What does GitHub do for us?

GitHub is a hosting platform for Git repositories. You can put your own
Git repositories on GitHub, access them from anywhere, and share them
with people around the world.

Beyond hosting repositories, GitHub also provides additional
collaboration features that are not native to Git (but are extremely
useful). Basically, GitHub helps people share and collaborate on
repositories.

------------------------------------------------------------------------

# Git vs GitHub

## Git

Git is a version control software that runs locally on your machine.

-   You don't need to register for an account.
-   You don't need the internet to use it.
-   You can use Git without ever touching GitHub.

## GitHub

GitHub is a service that hosts Git repositories in the cloud and makes
it easier to collaborate with other people.

-   You need to sign up for an account to use GitHub.
-   It is an online place to share work that is done using Git.

  -----------------------------------------------------------------------
  Git                         GitHub
  --------------------------- -------------------------------------------
  Local version control       Cloud hosting platform for Git repositories
  software                    

  Works offline               Requires internet for online features

  No account required         GitHub account required

  Tracks file changes         Enables collaboration, pull requests,
                              issues, and code reviews
  -----------------------------------------------------------------------

------------------------------------------------------------------------

# GitHub is Not Your Only Option

There are many competing tools that provide similar hosting and
collaboration features, including:

-   GitLab
-   Bitbucket
-   Gerrit

------------------------------------------------------------------------

# It's Free

GitHub offers its basic services for free.

While GitHub does offer paid Team and Enterprise plans, the basic free
tier includes:

-   Unlimited public repositories
-   Unlimited private repositories
-   Unlimited collaboration
-   GitHub Issues
-   GitHub Projects
-   Pull Requests
-   GitHub Actions (free usage limits)
-   GitHub Pages (for hosting static websites)

------------------------------------------------------------------------

# Why You Should Use GitHub

If you ever plan on working on a project with at least one other person,
GitHub will make your life much easier.

Whether you're building a hobby project with your friends or
collaborating with developers around the world, GitHub is an essential
tool.

Some benefits include:

-   Easy code sharing
-   Version history
-   Backup in the cloud
-   Team collaboration
-   Code reviews
-   Issue tracking
-   Project management
-   Continuous Integration (GitHub Actions)

------------------------------------------------------------------------

# Open Source Projects

Today GitHub is the home of many open source projects on the internet.

Popular projects hosted on GitHub include:

-   React
-   Node.js
-   Swift
-   VS Code
-   Kubernetes
-   TensorFlow
-   Flutter
-   Linux (mirror repositories)

Millions of developers contribute to open source software through GitHub
every day.

------------------------------------------------------------------------

# Cloning GitHub Repositories with `git clone`

Reference:

-   https://git-scm.com/docs/git-clone

## Cloning

So far we've created our own Git repositories from scratch.

But often we want to get a local copy of an existing repository instead.

To do this, we can clone a remote repository hosted on GitHub (or
similar websites).

All we need is the repository URL.

## Syntax

``` bash
git clone <url>
```

Example:

``` bash
git clone https://github.com/user/repository.git
```

## What Happens During Cloning?

When you run:

``` bash
git clone <url>
```

Git will:

1.  Download all files from the repository.
2.  Copy them to your local machine.
3.  Initialize a new Git repository.
4.  Download the complete commit history.
5.  Configure the remote named **origin** automatically.

------------------------------------------------------------------------

# Cloning Non-GitHub Repositories

Git can clone repositories hosted on many platforms, including:

-   GitLab
-   Bitbucket
-   Azure DevOps
-   Self-hosted Git servers
-   Gerrit
-   Any Git server accessible through HTTPS or SSH

As long as you have the repository URL, Git can clone it.

------------------------------------------------------------------------

# Permissions

Anyone can clone a repository from GitHub provided the repository is
**public**.

You do **not** need to be the owner or a collaborator to clone a public
repository.

You only need the repository URL.

However, pushing your own changes back to a GitHub repository is a
different story.

You must have permission to push changes.

For private repositories, you must authenticate using:

-   Personal Access Token (PAT)
-   SSH Keys
-   Organization access
-   Collaborator permissions

------------------------------------------------------------------------

# Common Git Clone Commands

Clone using HTTPS:

``` bash
git clone https://github.com/user/repository.git
```

Clone using SSH:

``` bash
git clone git@github.com:user/repository.git
```

Clone into a custom folder:

``` bash
git clone https://github.com/user/repository.git my-project
```

Clone only the latest commit:

``` bash
git clone --depth 1 <url>
```

------------------------------------------------------------------------

# Key Takeaways

-   Git is the version control system.
-   GitHub is a cloud platform that hosts Git repositories.
-   Git works locally without the internet.
-   GitHub enables collaboration and sharing.
-   Anyone can clone a public repository.
-   Permission is required to push changes.
-   `git clone` copies a repository to your local machine.
-   Cloning also downloads the full commit history by default.

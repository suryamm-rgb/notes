# GitHub Grab Bag — Odds and Ends

A collection of smaller but useful GitHub features: repo visibility,
collaborators, README files, Gists, and GitHub Pages.

---

## 1. Repo Visibility: Public vs. Private

### Public repos
- Accessible to **everyone** on the internet
- Anyone can view the code, even without a GitHub account

### Private repos
- Only accessible to the **owner** and people who have been **explicitly
  granted access**
- Not visible in search or to the general public

---

## 2. Adding GitHub Collaborators

If a repo is private (or even public, if you want others to have push
access), you can add collaborators so more than one person can work on it.

**Steps:**

1. Go to your repository on GitHub
2. Click **Settings**
3. Select **Collaborators** (under "Access")
4. Click **Add people** / **Invite a collaborator**
5. Search for their GitHub username or email and send the invite

The invited person will get a notification/email and must **accept** the
invite before they get access.

> This lets more than one person contribute to the same repository with
> proper permissions, instead of everyone working off separate copies.

---

## 3. README Files

A **README** file is used to communicate important information about a
repository, such as:

- **What** the project does
- **How** to run the project
- **Why** it's noteworthy
- **Who** maintains the project

### Key facts

- If you place a `README.md` in the **root** of your project, GitHub
  automatically detects it and displays it on the repo's home page.
- README files are written in **Markdown** (`.md` extension).
- Markdown is a lightweight, easy-to-learn syntax for formatting text
  (headings, bold, lists, links, code blocks, etc.).

**Minimal README example:**

```markdown
# Project Name

A short description of what this project does and who it's for.

## Installation

\`\`\`bash
npm install
\`\`\`

## Usage

\`\`\`bash
npm start
\`\`\`

## Maintainers

- Your Name (@yourhandle)
```

---

## 4. GitHub Gists

**Gists** are a simple way to share code snippets or useful fragments with
others.

- Much **quicker/easier to create** than a full repository
- But offer **far fewer features** (no issues, wikis, project boards, etc.)
- Every gist is its own Git repository, so it can still be cloned, forked,
  and commented on
- Can be created as **public** or **secret** (unlisted)

**Create one here:** https://gist.github.com/

Good for: one-off code snippets, config examples, quick sharing in chat or
forums — not for full projects.

---

## 5. GitHub Pages

**GitHub Pages** lets you host and publish a **public website** directly
from a GitHub repository.

- Simply push your site's code (HTML/CSS/JS, or a static site generator
  output) to GitHub
- GitHub builds and serves it as a live webpage
- Great for: project documentation, portfolios, personal blogs, or landing
  pages for open-source projects
- Typical URL pattern: `https://<username>.github.io/<repo-name>/`

---

## Quick Reference Table

| Feature | Purpose | Access Level |
|---|---|---|
| Public repo | Open-source / visible project | Anyone can view |
| Private repo | Restricted project | Owner + invited collaborators |
| Collaborators | Give others push access | Settings → Collaborators |
| README.md | Explain the project | Auto-displayed on repo home page |
| Gist | Share a quick code snippet | Public or secret |
| GitHub Pages | Host a static website | Public webpage from repo content |

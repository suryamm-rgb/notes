# Git Commits in Detail

## 1. Keeping Your Commits Atomic

An **atomic commit** should contain a single feature, bug fix, or change.

### Benefits
- Easier to review
- Easier to roll back
- Cleaner Git history
- Easier bug tracking

## Commit Messages

Use the **imperative (present) tense**.

✅ Good:
- Add login page
- Fix navbar
- Update README

❌ Avoid:
- Added login page
- Fixed navbar
- I changed login page

## Creating Commits

```bash
git add .
git commit
```

If Vim opens:
- Press `i` to insert.
- Type the message.
- Press `Esc`.
- Type `:wq` and press Enter.

## Use VS Code as Git Editor

```bash
git config --global core.editor "code --wait"
```

Install the `code` command:
1. Open VS Code.
2. Cmd+Shift+P (Mac) or Ctrl+Shift+P (Windows).
3. Run **Shell Command: Install 'code' command in PATH**.

## git log Commands

### git log
Shows full commit history.

### git log --abbrev-commit
Shows shortened commit hashes for easier reading.

### git log --oneline
Shows one commit per line with a short hash.

## Committing with a GUI

GUI tools let you stage files, write commit messages, and commit changes visually.

## Amending Commits

```bash
git commit -m "some commit"
git add forgotten_file
# or
git add .
git commit --amend
```

Use `--amend` to update the most recent commit instead of creating a new one.

## Ignoring Files

Create a `.gitignore` file in the repository root.

Examples:

```text
.DS_Store
folderName/
*.log
node_modules/
.env
```

Common files to ignore:
- API keys
- Credentials
- Log files
- Operating system files
- Dependencies
- Build output

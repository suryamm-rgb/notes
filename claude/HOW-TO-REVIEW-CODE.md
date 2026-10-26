# How to Review a Branch with the code-reviewer Agent

The agent is created once and lives at `~/.claude/agents/code-reviewer.md`.
It works in every project. You only repeat the steps below for each review.

---

## Step 1: Open the project in the terminal

```bash
cd ~/Desktop/code/vidyasagar-pattem
```

## Step 2: Update your branches

```bash
git fetch --all
git status
```

Make sure you have no uncommitted work you care about before switching branches.

## Step 3: See the branch names

```bash
git branch
```

Example branches: `feat/PS-298`, `fix/PS-320`, `main`, `test`.

## Step 4: Start Claude Code

```bash
claude
```

## Step 5: Ask for the review

Review one branch:

```
Use the code-reviewer subagent to review the branch fix/PS-320 and save it as REVIEW-fix-PS-320.md
```

Review several branches, one file each:

```
Use the code-reviewer subagent to review feat/PS-298, feat/PS-295 and fix/PS-320, and save each review in its own .md file inside a reviews/ folder
```

Compare against a different base branch:

```
Use the code-reviewer subagent to review feat/PS-298 compared to test
```

## Step 6: Wait for it to finish

- The agent runs in the background (about 30 to 60 seconds).
- Press `Ctrl+O` to see progress.
- Claude tells you the file name when it is done.

## Step 7: Read the review

The report is grouped into three levels:

| Level | Meaning | What to do |
|---|---|---|
| **Critical** | Bugs, security problems | Fix before merging |
| **Warnings** | Likely problems | Fix or check manually |
| **Suggestions** | Style, cleanup | Optional |

Open the file:

```bash
open REVIEW-fix-PS-320.md
```

## Step 8: Fix the issues

Ask Claude directly:

```
Fix the Critical and Warning items from REVIEW-fix-PS-320.md
```

## Step 9: Check what the agent cannot check

The agent only reads code. It does **not** run the app. Do these yourself:

- Start the app and open the changed pages
- Test the layout at 375, 768, 1024, 1440 and 1920 px if the change is visual
- Run tests and lint:
  ```bash
  npm run lint
  npm test
  ```

## Step 10: Clean up

The review file is untracked. Either delete it or ignore it:

```bash
echo "REVIEW-*.md" >> .gitignore
echo "reviews/" >> .gitignore
```

---

## Tips

- **Branch cut from `test`, not `main`?** Review against `test`, otherwise the diff includes all the old `test` commits (this happened with `fix/PS-320`).
- **Pull requests:** Target the same branch you compared against.
- **Similar names:** `feat/PE-295` and `feat/PS-295` look like a typo of each other. Confirm which one is correct.
- **Add your own rules:** Edit `~/.claude/agents/code-reviewer.md` to add team conventions. Every future review follows them.
- **Hidden folder:** `.claude` does not show in Finder search. Use `open ~/.claude/agents` or press `Cmd + Shift + .` in Finder.

## Quick reference

```bash
cd ~/Desktop/code/vidyasagar-pattem
git branch
claude
# then:
# Use the code-reviewer subagent to review <branch> and save it as REVIEW-<branch>.md
```

# GitLab CI/CD Mastery: Learn by Doing (Beginner → Advanced)

> A hands-on learning path covering **Linux, Git, GitLab CI/CD, automated testing, Docker, deployment, and AWS**.
> You will build **one real project** from `git init` to a production deployment on AWS, with every skill introduced exactly when the project needs it.

---

## How to use this document

1. **Do the labs, don't just read.** Every level ends with *Knowledge Check* questions and hands-on *Labs*. A level is "done" only when its labs pass their **✅ Done when** checks.
2. **Use one running project.** You create a small Node.js API in Level 2 and carry it through every level. Nothing is thrown away.
3. **Break things on purpose.** Industry skill comes from reading failed pipelines. Many labs ask you to make a pipeline fail, then fix it.
4. **Keep a learning log.** After each lab write 3 lines: *what I did, what broke, what I learned.* Put it in a `LEARNING_LOG.md` in your repo.
5. **Answers are hidden** in `<details>` blocks. Try first, then open them.

### Symbols used

| Symbol | Meaning |
|---|---|
| 💻 | Run in a terminal |
| 📄 | Create or edit a file |
| 🌐 | Do in a web UI (GitLab / AWS console) |
| ✅ | Acceptance check for a lab |
| 🚀 | Stretch goal |
| ⚠️ | Cost or security warning |
| 🧠 | Knowledge-check question |

---

## Contents

- [Learning roadmap](#learning-roadmap)
- [Level 0: Setup](#level-0-setup)
- [Level 1: Foundations: Linux and Git](#level-1-foundations-linux-and-git)
- [Level 2: GitLab CI/CD basics](#level-2-gitlab-cicd-basics)
- [Level 3: Automated testing, quality and security](#level-3-automated-testing-quality-and-security)
- [Level 4: Docker in the pipeline](#level-4-docker-in-the-pipeline)
- [Level 5: Deploying to a server](#level-5-deploying-to-a-server)
- [Level 6: AWS](#level-6-aws)
- [Level 7: Advanced CI/CD](#level-7-advanced-cicd)
- [Real-world workflows](#real-world-workflows)
- [Capstone project](#capstone-project)
- [Interview-style questions](#interview-style-questions)
- [Pipeline debugging playbook](#pipeline-debugging-playbook)
- [Cheat sheets](#cheat-sheets)
- [Progress tracker](#progress-tracker)

---

## Learning roadmap

```mermaid
flowchart LR
    L0["Level 0<br/>Setup"] --> L1["Level 1<br/>Linux and Git"]
    L1 --> L2["Level 2<br/>GitLab CI basics"]
    L2 --> L3["Level 3<br/>Testing and security"]
    L3 --> L4["Level 4<br/>Docker"]
    L4 --> L5["Level 5<br/>Deploy to a server"]
    L5 --> L6["Level 6<br/>AWS"]
    L6 --> L7["Level 7<br/>Advanced"]
    L7 --> CAP["Capstone<br/>Production pipeline"]
```

### Suggested 8-week schedule (about 8 to 10 hours per week)

| Week | Focus | Labs |
|---|---|---|
| 1 | Level 0 and Level 1: Linux, Git | 1 to 4 |
| 2 | Level 2: pipelines, jobs, variables, cache | 5 to 9 |
| 3 | Level 3: tests, coverage, services, security scans | 10 to 13 |
| 4 | Level 4: Docker, registry, image scanning | 14 to 17 |
| 5 | Level 5: Linux server, SSH deploy, rollback | 18 to 20 |
| 6 | Level 6: AWS IAM, OIDC, ECR, ECS | 21 to 23 |
| 7 | Level 6 and 7: secrets and alarms, S3, shared templates, review apps, releases | 24 to 27 |
| 8 | Level 7 and Capstone: Terraform, blue/green, supply chain, DORA, game day | 28 to 31, Capstone |

### What "industry level" means here

By the end you should be able to:

- Explain and operate a pipeline with **quality gates**, **security scans**, **artifact versioning**, **environment promotion**, and **rollback**.
- Authenticate CI to AWS **without long-lived keys** (OIDC).
- Debug a failing pipeline from logs alone.
- Reduce pipeline time and cost, and measure delivery performance (DORA metrics).

---

## Level 0: Setup

### 0.1 Accounts you need

| Account | Why | Notes |
|---|---|---|
| **GitLab.com** (free) | Repos, pipelines, registry | Shared runners have a monthly compute-minute quota that depends on your plan. New accounts may need identity verification before using shared runners. Check your current limits under *Settings → Usage Quotas*. |
| **AWS** | Levels 5 and 6 | A credit card is required. ⚠️ AWS free-tier rules have changed over time, so check the Billing console to see what applies to your account. |
| **Docker Hub** (optional) | Pulling base images | Anonymous pulls work but are rate-limited. |

### ⚠️ 0.2 AWS safety setup (do this BEFORE anything else on AWS)

1. 🌐 **Enable MFA** on the root user. Then **stop using root**.
2. 🌐 Create an **IAM Identity Center user** (or an IAM user) with admin rights for *learning only*, and use that for daily work.
3. 🌐 **Billing → Budgets → Create budget**: a monthly cost budget of **$10** with email alerts at 50%, 80%, 100%.
4. 🌐 Pick **one region** (for example `ap-south-1` Mumbai, close to you) and use it for everything.
5. Keep a `CLEANUP.md` listing every resource you create. At the end of each study session you will delete what you do not need.

Common surprise bills: **NAT Gateways**, **Application Load Balancers**, **RDS instances**, **Elastic IPs not attached**, **forgotten EC2/Fargate tasks**. Level 6 tells you how to avoid each.

### 0.3 Local tools

💻 Check what you have:

```bash
git --version        # 2.30+
node --version       # 20.9+ (22 recommended)
npm --version
docker --version     # Docker Desktop or Docker Engine
docker compose version
ssh -V
curl --version
```

Install anything missing:

| Tool | Install |
|---|---|
| Git | https://git-scm.com |
| Node.js | https://nodejs.org (LTS) or `nvm` |
| Docker | https://docs.docker.com/get-docker/ |
| AWS CLI v2 | https://docs.aws.amazon.com/cli/latest/userguide/getting-started-install.html |
| VS Code | https://code.visualstudio.com |
| Windows users | Install **WSL2 + Ubuntu**. Do all labs inside the Ubuntu terminal. |

### 0.4 Configure Git and SSH keys

💻

```bash
git config --global user.name  "Your Name"
git config --global user.email "you@example.com"
git config --global init.defaultBranch main
git config --global pull.rebase true

# SSH key for GitLab (press Enter for defaults; set a passphrase)
ssh-keygen -t ed25519 -C "you@example.com"
cat ~/.ssh/id_ed25519.pub        # copy this
```

🌐 GitLab → **Avatar → Preferences → SSH Keys → Add new key** → paste.

💻 Test:

```bash
ssh -T git@gitlab.com            # "Welcome to GitLab, @yourname!"
```

### 0.5 Create the project

🌐 GitLab → **New project → Create blank project**
- Name: `bookshelf-api`
- Visibility: **Private**
- Uncheck "Initialize with a README"

💻

```bash
mkdir bookshelf-api && cd bookshelf-api
git init
git remote add origin git@gitlab.com:YOUR_USERNAME/bookshelf-api.git
echo "# bookshelf-api" > README.md
git add . && git commit -m "chore: initial commit"
git push -u origin main
```

> You will build the actual application in **Level 2**. First, the foundations.

---

## Level 1: Foundations: Linux and Git

CI/CD runs on Linux, deploys to Linux, and is driven by Git. Weak fundamentals here cause most pipeline pain later.

### 1.1 Linux essentials

#### Navigation and files

```bash
pwd                         # where am I
ls -lah                     # list all, human-readable
cd /var/log && cd ~ && cd - # absolute, home, previous dir
mkdir -p app/{src,tests,logs}     # create nested folders
touch app/src/index.js
cp -r app app-backup
mv app-backup /tmp/
rm -rf /tmp/app-backup      # ⚠️ permanent, no trash can
cat file.txt                # print file
less file.txt               # scroll (q to quit)
head -n 20 file.txt && tail -n 20 file.txt
tail -f /var/log/syslog     # follow a log live (Ctrl+C to stop)
```

#### Searching and text processing (daily CI/CD skills)

```bash
grep -rn "TODO" src/                 # recursive search with line numbers
grep -i "error" app.log | wc -l      # count error lines
find . -name "*.test.js" -not -path "./node_modules/*"
awk '{print $1}' access.log | sort | uniq -c | sort -rn | head   # top IPs
sed -i 's/old/new/g' file.txt        # in-place replace
cut -d',' -f2 data.csv
```

Pipes `|` send one command's output to the next. Redirects: `>` overwrite, `>>` append, `2>&1` merge errors into output.

#### Permissions and ownership

```bash
ls -l script.sh
# -rwxr-xr-- 1 alice dev 120 Nov 24 10:00 script.sh
#  │└┬┘└┬┘└┬┘   owner group
#  │ │  │  └ others: r--  (4)
#  │ │  └ group:  r-x      (5)
#  │ └ owner:  rwx         (7)   → chmod 754
#  └ file type (- file, d directory)

chmod +x script.sh          # make executable
chmod 600 ~/.ssh/id_ed25519 # private keys must be owner-only
chown deploy:deploy /opt/app
sudo command                # run as administrator
```

| Number | Meaning |
|---|---|
| 4 | read |
| 2 | write |
| 1 | execute |
| 755 | owner rwx, others r-x (typical for scripts and folders) |
| 644 | owner rw, others r (typical for files) |
| 600 | owner only (secrets, SSH keys) |

#### Processes, ports, disk, memory

```bash
ps aux | grep node          # find a process
top   # or: htop            # live CPU/RAM
kill 1234 && kill -9 1234   # polite stop, force stop
df -h                       # disk space
du -sh * | sort -h          # which folders are big
free -m                     # memory
ss -tulpn                   # which process listens on which port
curl -i http://localhost:3000/health
```

#### Services and logs (systemd)

```bash
sudo systemctl status docker
sudo systemctl restart docker
sudo systemctl enable docker        # start on boot
journalctl -u docker -n 100 --no-pager
journalctl -u docker -f             # follow live
```

#### Environment variables

```bash
export APP_ENV=staging
echo "$APP_ENV"
env | grep APP_
# Per command only:
APP_ENV=test node app.js
```

#### Writing safe Bash scripts

📄 `scripts/hello.sh`

```bash
#!/usr/bin/env bash
set -euo pipefail        # exit on error, on unset variable, on pipe failure

NAME="${1:-world}"       # first argument, default "world"

log() { echo "[$(date +%H:%M:%S)] $*"; }

log "Hello, ${NAME}!"

if [ ! -d "./reports" ]; then
  log "Creating reports folder"
  mkdir -p ./reports
fi

for f in ./*.md; do
  log "Found markdown file: $f"
done
```

```bash
chmod +x scripts/hello.sh
./scripts/hello.sh Alice
```

> Always start CI scripts with `set -euo pipefail`. Without it, a failed command in the middle of a script can be silently ignored and your pipeline will go green with broken output.

#### SSH essentials

```bash
ssh user@1.2.3.4                       # login
ssh -i ~/.ssh/mykey.pem ec2-user@1.2.3.4
scp file.txt user@host:/tmp/           # copy a file to a server
ssh user@host "docker ps"              # run one command remotely
```

`~/.ssh/config` makes this shorter:

```text
Host staging
  HostName 1.2.3.4
  User deploy
  IdentityFile ~/.ssh/id_ed25519
```

Now `ssh staging` just works.

### 🧠 Knowledge Check: Linux

1. What does `chmod 640 secrets.env` allow for owner, group, and others?
2. What is the difference between `>` and `>>`?
3. A pipeline script runs `cd missing_folder && rm -rf *`. Why is `set -e` plus correct quoting important here?
4. How do you find which process is using port 3000?
5. What does `set -euo pipefail` do? Explain each letter.
6. Why must SSH private keys be `chmod 600`?
7. How would you count how many lines in `app.log` contain "ERROR"?
8. What is the difference between `kill` and `kill -9`?

<details>
<summary>Answers</summary>

1. Owner read+write, group read only, others nothing.
2. `>` overwrites the file; `>>` appends.
3. If `cd` fails and the script continues, `rm -rf *` would run in the *current* folder and delete the wrong files. `set -e` stops the script on the first failure. (Chaining with `&&` also protects this one command.)
4. `ss -tulpn | grep 3000` or `lsof -i :3000`.
5. `-e` exit on any command failure; `-u` treat unset variables as errors; `-o pipefail` make a pipeline fail if any command in it fails, not only the last.
6. SSH refuses to use keys readable by others, because anyone with read access could impersonate you.
7. `grep -c "ERROR" app.log` (or `grep "ERROR" app.log | wc -l`).
8. `kill` sends SIGTERM (a polite request to shut down gracefully); `kill -9` sends SIGKILL (immediate, no cleanup).
</details>

### 1.2 Git essentials

Git tracks snapshots of your project. Understand the **three areas**:

```text
Working directory  ──git add──▶  Staging area  ──git commit──▶  Local repo  ──git push──▶  Remote (GitLab)
```

#### Daily commands

```bash
git status
git add file.js              # stage one file (git add . stages everything)
git commit -m "feat: add login"
git log --oneline --graph --decorate --all
git diff                     # unstaged changes
git diff --staged            # staged changes
git restore file.js          # discard working changes
git restore --staged file.js # unstage
```

#### Branching

```bash
git switch -c feat/search          # create + switch
git switch main
git branch -d feat/search          # delete merged branch
git push -u origin feat/search
git fetch origin                   # download without merging
git pull                           # fetch + integrate
```

#### Merge vs rebase

```text
merge:   A──B──C──────M      (keeps both lines of history, adds a merge commit)
              \      /
               D──E

rebase:  A──B──C──D'──E'     (replays your commits on top of the new base: linear history)
```

```bash
git switch feat/search
git rebase main                    # update your branch on top of main
# if conflicts: edit files → git add <file> → git rebase --continue
```

> **Golden rule:** never rebase commits that others already pulled.

#### Resolving a merge conflict

Git marks conflicts like this:

```text
<<<<<<< HEAD
const port = 3000;
=======
const port = process.env.PORT ?? 3000;
>>>>>>> feat/config
```

Edit the file to the final correct content, delete the markers, then:

```bash
git add src/server.js
git rebase --continue      # or: git commit (for a merge)
```

#### Undoing things safely

| Situation | Command |
|---|---|
| Fix the last commit message (not pushed) | `git commit --amend` |
| Undo last commit, keep changes | `git reset --soft HEAD~1` |
| Safely undo a pushed commit | `git revert <sha>` (creates a new commit) |
| Save work temporarily | `git stash push -m "wip"` / `git stash pop` |
| Copy one commit to another branch | `git cherry-pick <sha>` |
| "I lost a commit!" | `git reflog` then `git switch -c rescue <sha>` |

#### Tags (releases)

```bash
git tag -a v1.0.0 -m "First release"
git push origin v1.0.0
```

#### Conventional Commits

Format: `type(scope): description`. Types: `feat`, `fix`, `refactor`, `docs`, `test`, `chore`, `ci`, `build`, `perf`. Breaking change: `feat(api)!: remove v1`.

#### `.gitignore`

📄 `.gitignore`

```text
node_modules/
coverage/
reports/
dist/
.env
.env.*
!.env.example
*.log
.DS_Store
```

#### Branching strategies you will meet at work

| Strategy | Idea | Best for |
|---|---|---|
| **Trunk-based** | Everyone merges small changes into `main` several times a day | Mature teams with strong tests, SaaS |
| **GitFlow** | `main`, `develop`, `feature/*`, `release/*`, `hotfix/*` | Versioned software with scheduled releases |
| **Environment branches** | `test → stage → main` map to environments | Teams wanting explicit promotion gates (used in this guide) |

### 🧠 Knowledge Check: Git

1. What is the difference between `git fetch` and `git pull`?
2. You committed a password and pushed it. Is `git revert` enough? What must you do?
3. When would you choose `git rebase` over `git merge`? When not?
4. What does `git reset --hard HEAD~1` do, and why is it dangerous?
5. How do you recover a branch you deleted by accident?
6. Why do teams use squash merges for feature branches?
7. What is the staging area for?

<details>
<summary>Answers</summary>

1. `fetch` only downloads remote changes; `pull` fetches **and** integrates them (merge or rebase) into your current branch.
2. No. The secret stays in history and is already exposed. **Rotate/revoke the password immediately**; optionally rewrite history with `git filter-repo` and force-push, but treat the secret as compromised either way.
3. Rebase to keep your *own* feature branch up to date with a clean linear history. Do not rebase shared or already-pulled branches.
4. It moves the branch back one commit and **discards** the changes in your working directory. Uncommitted work is lost (committed work can be found via `reflog`).
5. `git reflog` to find the last commit SHA of that branch, then `git switch -c branch-name <sha>`.
6. One clean commit per feature; simpler history and easier reverts.
7. It lets you choose exactly which changes go into the next commit.
</details>

### 🛠️ Labs: Level 1

**Lab 1: Linux survival kit**

- **Goal:** Get comfortable in a Linux shell.
- **Steps:**
  1. Create `~/lab1/{src,logs,backup}`.
  2. Generate a fake log: `for i in $(seq 1 200); do echo "$(date +%F) $((RANDOM%3 == 0 ? 1 : 0)) request $i" >> ~/lab1/logs/app.log; done` and add 20 lines containing `ERROR` with `echo`.
  3. Count the `ERROR` lines, show the last 5 of them, and save the result to `~/lab1/report.txt`.
  4. Create `deploy.sh`, make it executable (`chmod 750`), and verify with `ls -l`.
  5. Find the 5 largest files under `/usr` with `du` or `find`.
  6. Start `python3 -m http.server 8000 &`, find its PID with `ps` and its port with `ss`, then stop it with `kill`.
- ✅ **Done when:** `report.txt` exists with the correct counts, and you can explain every command you used.
- 🚀 **Stretch:** Write a one-liner that prints the top 3 most frequent words in a text file.

**Lab 2: Bash log analyzer**

- **Goal:** Write a robust script you could run in a pipeline.
- **Steps:** Create `scripts/analyze.sh` that:
  1. takes a log file path as `$1` (exit with code `2` and a usage message if missing or the file does not exist),
  2. prints total lines, number of `ERROR` and `WARN` lines,
  3. exits with code `1` if the error rate is above 10%, otherwise `0`,
  4. starts with `set -euo pipefail`.
- ✅ **Done when:** `./analyze.sh` with no argument exits `2`; with a clean log exits `0`; with an error-heavy log exits `1` (check with `echo $?`).
- 🚀 **Stretch:** Add `--max-error-rate=N` as an option.

**Lab 3: Git branching, conflicts, and history**

- **Goal:** Practice the workflow you will use every day.
- **Steps:**
  1. In `bookshelf-api` create branches `test` and `stage` from `main` and push all three.
  2. Create `feat/a` and `feat/b` from `test`. Both edit **the same line** of `README.md` differently and push.
  3. Merge `feat/a` into `test`. Rebase `feat/b` onto `test`, **resolve the conflict**, finish the rebase.
  4. Make 3 messy commits on a branch, squash them into 1 with `git rebase -i HEAD~3`.
  5. Create a tag `v0.1.0` and push it.
- ✅ **Done when:** `git log --oneline --graph --all` shows your history, the conflict is resolved, and the tag is visible in GitLab under *Code → Tags*.
- 🚀 **Stretch:** Protect `main` in GitLab (*Settings → Repository → Protected branches*) so direct pushes are blocked, then try to push to it and read the error.

**Lab 4: Undo disasters**

- **Goal:** Never fear a Git mistake again.
- **Steps:**
  1. Make a commit, then `git reset --hard HEAD~1`. Recover the commit using `git reflog`.
  2. Delete a branch with unmerged commits (`git branch -D`) and restore it.
  3. Commit a fake secret `AWS_SECRET=abc123` to a file, push it, then fix it properly: remove the file, add it to `.gitignore`, and write down in `LEARNING_LOG.md` what a real rotation would involve.
  4. Use `git stash` to switch branches mid-work and come back.
- ✅ **Done when:** you restored both the commit and the branch using only `reflog`, and can explain why step 3's `git revert` alone is not enough.

---

## Level 2: GitLab CI/CD basics

### 2.1 Concepts (read once, then build)

| Term | Meaning |
|---|---|
| **CI** (Continuous Integration) | Every push is automatically built and tested so integration problems show up within minutes. |
| **Continuous Delivery** | Every passing change is *ready* to release; a human clicks the button. |
| **Continuous Deployment** | Every passing change is released automatically, no human click. |
| **Pipeline** | The whole automated run for one commit or merge request. |
| **Stage** | A group of jobs that run in parallel; stages run in order. |
| **Job** | One unit of work (a script) that runs on a runner. |
| **Runner** | The machine or container that executes jobs. GitLab.com provides shared runners; you can also register your own. |
| **Artifact** | Files a job produces and passes on or stores (build output, test reports). |
| **Cache** | Files reused between pipeline runs to save time (for example `node_modules`). |
| **Environment** | A named deployment target (`staging`, `production`) tracked by GitLab. |

```mermaid
flowchart LR
    PUSH["git push / Merge Request"] --> S1
    subgraph PL["Pipeline"]
        direction LR
        S1["Stage: build<br/>job: install"] --> S2["Stage: test<br/>jobs: lint, unit (parallel)"] --> S3["Stage: deploy<br/>job: deploy-staging"]
    end
```

**Cache vs artifact:** a *cache* is a best-effort speed-up that may be missing; an *artifact* is a guaranteed output handed to later jobs and downloadable from the UI. Never rely on cache for correctness.

### 2.2 Build the application you will carry through the whole course

You will create a tiny Express API called `bookshelf-api`. The point is the pipeline, not the app, so it stays small.

💻 From inside your `bookshelf-api` folder:

```bash
git switch -c feat/initial-app
npm init -y
npm pkg set type=module
npm pkg set engines.node=">=20"
npm install express
npm install -D vitest @vitest/coverage-v8 supertest eslint @eslint/js globals prettier
mkdir -p src tests
```

📄 `src/books.js`: the in-memory data layer

```js
import { randomUUID } from "node:crypto";

const books = new Map();

export function listBooks() {
  return [...books.values()];
}

export function getBook(id) {
  return books.get(id);
}

export function addBook({ title, author }) {
  if (typeof title !== "string" || title.trim() === "") {
    throw new Error("title is required");
  }
  if (typeof author !== "string" || author.trim() === "") {
    throw new Error("author is required");
  }
  const book = { id: randomUUID(), title: title.trim(), author: author.trim() };
  books.set(book.id, book);
  return book;
}

export function removeBook(id) {
  return books.delete(id);
}

export function clearBooks() {
  books.clear();
}
```

📄 `src/app.js`: the Express app (exported so tests can use it without opening a port)

```js
import express from "express";
import { addBook, getBook, listBooks, removeBook } from "./books.js";

export function createApp() {
  const app = express();
  app.use(express.json());

  app.get("/health", (_req, res) => {
    res.json({ status: "ok", version: process.env.APP_VERSION ?? "dev" });
  });

  app.get("/books", (_req, res) => res.json(listBooks()));

  app.get("/books/:id", (req, res) => {
    const book = getBook(req.params.id);
    if (!book) return res.status(404).json({ error: "not found" });
    res.json(book);
  });

  app.post("/books", (req, res) => {
    try {
      res.status(201).json(addBook(req.body ?? {}));
    } catch (err) {
      res.status(400).json({ error: err.message });
    }
  });

  app.delete("/books/:id", (req, res) => {
    if (!removeBook(req.params.id)) return res.status(404).json({ error: "not found" });
    res.status(204).end();
  });

  return app;
}
```

📄 `src/server.js`: the entry point, with graceful shutdown

```js
import { createApp } from "./app.js";

const port = Number(process.env.PORT ?? 3000);
const server = createApp().listen(port, () => {
  console.log(JSON.stringify({ level: "info", msg: "listening", port }));
});

function shutdown(signal) {
  console.log(JSON.stringify({ level: "info", msg: "shutting down", signal }));
  server.close(() => process.exit(0));
  setTimeout(() => process.exit(1), 10_000).unref();
}

process.on("SIGTERM", () => shutdown("SIGTERM"));
process.on("SIGINT", () => shutdown("SIGINT"));
```

📄 `tests/books.test.js`

```js
import request from "supertest";
import { beforeEach, describe, expect, it } from "vitest";
import { createApp } from "../src/app.js";
import { clearBooks } from "../src/books.js";

const app = createApp();

describe("bookshelf API", () => {
  beforeEach(() => clearBooks());

  it("reports health", async () => {
    const res = await request(app).get("/health");
    expect(res.status).toBe(200);
    expect(res.body.status).toBe("ok");
  });

  it("creates and lists a book", async () => {
    const created = await request(app)
      .post("/books")
      .send({ title: "Dune", author: "Frank Herbert" });
    expect(created.status).toBe(201);

    const list = await request(app).get("/books");
    expect(list.body).toHaveLength(1);
    expect(list.body[0].title).toBe("Dune");
  });

  it("rejects a book without a title", async () => {
    const res = await request(app).post("/books").send({ author: "Nobody" });
    expect(res.status).toBe(400);
  });

  it("returns 404 for an unknown book", async () => {
    const res = await request(app).get("/books/does-not-exist");
    expect(res.status).toBe(404);
  });

  it("deletes a book", async () => {
    const { body } = await request(app)
      .post("/books")
      .send({ title: "Emma", author: "Jane Austen" });
    const del = await request(app).delete(`/books/${body.id}`);
    expect(del.status).toBe(204);
    const again = await request(app).delete(`/books/${body.id}`);
    expect(again.status).toBe(404);
  });
});
```

📄 `vitest.config.js`

```js
import { defineConfig } from "vitest/config";

export default defineConfig({
  test: {
    environment: "node",
    include: ["tests/**/*.test.js"],
    coverage: {
      provider: "v8",
      include: ["src/**/*.js"],
      exclude: ["src/server.js"],
      reporter: ["text", "text-summary", "cobertura", "html"],
      thresholds: { lines: 80, functions: 80, branches: 70, statements: 80 },
    },
  },
});
```

📄 `eslint.config.js`

```js
import js from "@eslint/js";
import globals from "globals";

export default [
  { ignores: ["coverage/**", "reports/**", "node_modules/**", "dist/**"] },
  js.configs.recommended,
  {
    languageOptions: { globals: { ...globals.node } },
    rules: { "no-unused-vars": ["error", { argsIgnorePattern: "^_" }] },
  },
];
```

📄 `.prettierrc.json`

```json
{ "semi": true, "singleQuote": false, "trailingComma": "all", "printWidth": 100 }
```

📄 `package.json`: set the scripts (replace the existing `"scripts"` block)

```json
"scripts": {
  "start": "node src/server.js",
  "dev": "node --watch src/server.js",
  "lint": "eslint . --max-warnings=0",
  "format:check": "prettier --check .",
  "format": "prettier --write .",
  "test": "vitest run",
  "test:coverage": "vitest run --coverage"
}
```

💻 Make sure it all works locally before touching CI. **Rule: never debug in the pipeline what you can debug on your laptop.**

```bash
npm run format
npm run lint
npm run test:coverage
npm start        # visit http://localhost:3000/health, then Ctrl+C
```

💻 Commit and merge it:

```bash
git add .
git commit -m "feat: add bookshelf API with tests"
git push -u origin feat/initial-app
```

🌐 Open a Merge Request into `main` (create `test` and `stage` branches later in Lab 3 if you have not), merge it, then `git switch main && git pull`.

### 2.3 Your first pipeline

📄 `.gitlab-ci.yml`

```yaml
image: node:22-alpine

stages:
  - build
  - test

install:
  stage: build
  script:
    - node --version
    - npm ci
  artifacts:
    paths:
      - node_modules/
    expire_in: 1 hour

lint:
  stage: test
  script:
    - npm run lint

unit-tests:
  stage: test
  script:
    - npm run test
```

💻

```bash
git switch -c ci/first-pipeline
git add .gitlab-ci.yml
git commit -m "ci: add first pipeline"
git push -u origin ci/first-pipeline
```

🌐 GitLab → **Build → Pipelines**. Click the pipeline, click a job, and read the log top to bottom. Find: the Docker image that was pulled, the commands that ran, the exit code, the duration.

> **Reading a job log:** a job passes when every command in `script` exits with code `0`. The first non-zero exit stops the job and marks it failed.

### 2.4 The `.gitlab-ci.yml` building blocks

#### Image, scripts, and hooks

```yaml
default:
  image: node:22-alpine         # container every job uses unless overridden
  before_script:
    - npm ci --prefer-offline   # runs before every job's script
  interruptible: true           # newer pipelines on the same branch may cancel it

job:
  script:
    - echo "main work"
  after_script:
    - echo "always runs, even when script fails (cleanup)"
```

#### Variables

```yaml
variables:
  NODE_ENV: test                # pipeline-wide
  NPM_CONFIG_CACHE: "$CI_PROJECT_DIR/.npm"

job:
  variables:
    LOG_LEVEL: debug            # job-level, overrides pipeline-level
  script:
    - echo "Branch is $CI_COMMIT_REF_NAME, commit is $CI_COMMIT_SHORT_SHA"
```

Useful **predefined variables**:

| Variable | Example value |
|---|---|
| `CI_COMMIT_SHA`, `CI_COMMIT_SHORT_SHA` | full SHA, first 8 characters |
| `CI_COMMIT_BRANCH` | `main` (not set in tag or merge-request pipelines) |
| `CI_COMMIT_TAG` | `v1.2.0` (only in tag pipelines) |
| `CI_MERGE_REQUEST_IID`, `CI_MERGE_REQUEST_TARGET_BRANCH_NAME` | set only in merge request pipelines |
| `CI_DEFAULT_BRANCH` | `main` |
| `CI_PIPELINE_SOURCE` | `push`, `merge_request_event`, `schedule`, `web`, `api` |
| `CI_PROJECT_DIR` | `/builds/you/bookshelf-api` |
| `CI_REGISTRY`, `CI_REGISTRY_IMAGE`, `CI_REGISTRY_USER`, `CI_REGISTRY_PASSWORD` | the project's container registry and a job-scoped login |
| `CI_JOB_TOKEN` | short-lived token for API and registry access |
| `CI_ENVIRONMENT_NAME` | set when the job has an `environment` |

#### Secret variables (never put secrets in YAML)

🌐 **Settings → CI/CD → Variables → Add variable**.

| Option | Use it when |
|---|---|
| **Masked** | the value must be hidden in job logs (must meet GitLab's masking format rules) |
| **Protected** | only available to pipelines on protected branches/tags. Use for production secrets. |
| **Environment scope** | a different value for `staging` vs `production` |
| **Type: File** | the value is written to a temp file and the variable holds its path (ideal for SSH keys, kubeconfigs) |

> Masking is a safety net, not a vault. A job script can still print a secret by transforming it (base64, spaces). Restrict *who can edit CI config* on protected branches.

#### Cache

```yaml
default:
  cache:
    key:
      files:
        - package-lock.json      # new cache whenever dependencies change
    paths:
      - .npm/
    policy: pull-push
```

Pair with `npm ci --prefer-offline` and `NPM_CONFIG_CACHE=$CI_PROJECT_DIR/.npm`. Cache *downloaded packages* (`.npm/`) rather than `node_modules/`, because `npm ci` deletes `node_modules` anyway.

#### Artifacts and reports

```yaml
unit-tests:
  stage: test
  script:
    - npm run test:coverage -- --reporter=default --reporter=junit --outputFile.junit=reports/junit.xml
  coverage: '/All files[^|]*\|[^|]*\s+([\d\.]+)/'
  artifacts:
    when: always                 # keep reports even when tests fail
    expire_in: 7 days
    paths:
      - coverage/
    reports:
      junit: reports/junit.xml   # shows test results in the MR
      coverage_report:
        coverage_format: cobertura
        path: coverage/cobertura-coverage.xml
```

#### `rules`: decide when a job runs

```yaml
workflow:                          # decide when a PIPELINE exists at all
  rules:
    - if: $CI_PIPELINE_SOURCE == "merge_request_event"
    - if: $CI_COMMIT_BRANCH && $CI_OPEN_MERGE_REQUESTS
      when: never                  # avoid duplicate branch + MR pipelines
    - if: $CI_COMMIT_BRANCH
    - if: $CI_COMMIT_TAG

deploy-staging:
  rules:
    - if: $CI_COMMIT_BRANCH == "stage"            # only on stage
deploy-production:
  rules:
    - if: $CI_COMMIT_BRANCH == "main"
      when: manual                                 # human click required
docs-check:
  rules:
    - changes:
        - "**/*.md"                                # only when markdown changed
```

#### `needs`: run jobs out of stage order (DAG)

```yaml
unit-tests:
  stage: test
  needs: [install]       # starts as soon as "install" finishes, not when the whole build stage does
```

#### `extends`, anchors, and `include`: remove duplication

```yaml
.node-job:                         # hidden template (leading dot)
  image: node:22-alpine
  before_script:
    - npm ci --prefer-offline

lint:
  extends: .node-job
  stage: test
  script: [npm run lint]
```

```yaml
include:
  - local: ".gitlab/ci/test.yml"
  - template: Security/SAST.gitlab-ci.yml
  - project: "platform/ci-templates"
    ref: v2.0.0
    file: "/node.yml"
```

#### Other useful keywords

| Keyword | Purpose |
|---|---|
| `timeout` | fail a stuck job (`timeout: 10 minutes`) |
| `retry` | `retry: { max: 2, when: [runner_system_failure, stuck_or_timeout_failure] }` (retry infrastructure faults, never real test failures) |
| `allow_failure: true` | job may fail without failing the pipeline (a yellow warning) |
| `parallel: N` or `parallel: { matrix: [...] }` | run copies or a matrix (for example Node 20 and 22) |
| `resource_group` | allow only one deploy at a time to an environment |
| `environment` | track deployments, enable rollback and review apps |
| `tags` | pick runners by label (required for self-hosted runners) |
| `services` | sidecar containers (databases) next to the job |

#### Runners

- **Shared runners (GitLab.com):** fine for learning; ephemeral Docker containers, limited minutes.
- **Self-hosted runners:** you install `gitlab-runner` on your own machine or EC2, register it with a token, and pick an executor (usually **docker**). Use them for private networks, special hardware, or to avoid quotas. A runner that can build Docker images is a high-value target, so treat its host as sensitive.

### 🧠 Knowledge Check: CI/CD basics

1. What is the difference between Continuous Delivery and Continuous Deployment?
2. Jobs in the same stage: sequential or parallel? And stages?
3. When would you use `artifacts` and when `cache`?
4. Why should production secrets be both **Masked** and **Protected**?
5. A pipeline runs twice for every push on a branch that has an open MR. Why, and how do you stop it?
6. What does `needs:` change about scheduling?
7. Why would `retry: 2` on unit tests be a bad idea?
8. What does `when: always` on artifacts do, and why is it useful for reports?
9. What does `allow_failure: true` do? Name one good and one bad use.

<details>
<summary>Answers</summary>

1. Delivery: always releasable, a human triggers the release. Deployment: releases automatically after passing.
2. Jobs in a stage run in parallel (given enough runners); stages run in order, and a failed stage stops later ones (unless `allow_failure` or `needs` changes it).
3. Artifacts = guaranteed outputs passed to later jobs or downloaded (build output, reports). Cache = best-effort reuse of downloaded dependencies between runs.
4. Masked hides it in logs; Protected restricts it to protected branches/tags, so a random feature branch (or someone's modified CI file) cannot read it.
5. A branch pipeline *and* a merge-request pipeline are both created. Fix with `workflow:rules` that skip branch pipelines when an MR is open.
6. It builds a DAG: a job starts as soon as the jobs it needs finish, regardless of stage boundaries, so pipelines get faster.
7. It hides flaky tests, wastes minutes, and teaches the team to ignore red. Retry only infrastructure failures.
8. Uploads artifacts even if the job failed, so you can inspect test reports for the failure.
9. Pipeline continues if the job fails. Good: a non-blocking dependency audit. Bad: making a failing test "allowed" so the pipeline looks green.
</details>

### 🛠️ Labs: Level 2

**Lab 5: First pipeline and reading logs**

- **Steps:**
  1. Make the pipeline from section 2.3 run on your branch.
  2. Add a job `print-env` that prints `CI_COMMIT_SHORT_SHA`, `CI_COMMIT_BRANCH`, `CI_PIPELINE_SOURCE`, and `CI_PROJECT_DIR`.
  3. Break it on purpose: add `script: [exit 1]` to a job. Find the failure in the UI, then fix it.
  4. Add a lint error (`const x = 1;` unused) and watch `lint` fail. Fix it.
- ✅ **Done when:** you have one red and one green pipeline in history and can say where each failure message appeared.

**Lab 6: Variables and secrets**

- **Steps:**
  1. 🌐 Add a variable `DEMO_SECRET` (Masked). In a job, run `echo "$DEMO_SECRET"` and see what the log shows.
  2. Try to print it reversed: `echo "$DEMO_SECRET" | rev`. Write in your log why masking is not a vault.
  3. Add `DEPLOY_ENV` with environment scopes `staging` and `production` and different values. Print it from a job that has `environment: staging`.
  4. Make `DEMO_SECRET` **Protected** and confirm a feature-branch pipeline cannot read it (the variable will be empty).
- ✅ **Done when:** you can explain masked vs protected vs scoped with evidence from your own pipeline logs.

**Lab 7: Cache and speed**

- **Steps:**
  1. Add the cache config from section 2.4 and set `NPM_CONFIG_CACHE`.
  2. Run the pipeline twice. Compare the `npm ci` time of run 1 (cold) and run 2 (warm).
  3. Change a dependency (`npm install lodash`), push, and observe the cache key change.
  4. Use `needs:` so `lint` and `unit-tests` start immediately after `install`. Compare total pipeline duration.
- ✅ **Done when:** you recorded cold vs warm timing in `LEARNING_LOG.md`.
- 🚀 **Stretch:** Add `interruptible: true` and push two quick commits. Observe the older pipeline getting cancelled.

**Lab 8: Test reports, coverage, and MR integration**

- **Steps:**
  1. Configure JUnit + Cobertura artifacts (section 2.4) and the `coverage:` regex.
  2. Open a Merge Request. Find the **test summary** widget and the **coverage** badge or diff.
  3. Remove a test so coverage drops below the 80% threshold and observe the job fail (the threshold is set in `vitest.config.js`).
  4. 🌐 Add the coverage badge: **Settings → General → Badges**.
- ✅ **Done when:** a failing coverage threshold blocks the job, and test results appear in the MR UI.

**Lab 9: Rules and workflow**

- **Steps:**
  1. Add `workflow:rules` so only MR pipelines and pipelines on `test`/`stage`/`main` exist (no duplicates).
  2. Add a `docs-lint` job that runs only when `*.md` files change (`rules: changes`). Test with a README-only commit.
  3. Add a job that is `when: manual` and verify the play button.
  4. 🌐 **Settings → Merge requests → "Pipelines must succeed"**, then try merging an MR with a red pipeline.
- ✅ **Done when:** a docs-only change triggers `docs-lint` and nothing else heavy, and an MR with a failing pipeline cannot be merged.

---

## Level 3: Automated testing, quality and security

A pipeline is only as trustworthy as the checks inside it. This level turns your pipeline into a real **quality gate**.

### 3.1 The test pyramid

```text
            /\
           /E2E\         few, slow, expensive    (Playwright against a deployed URL)
          /------\
         /Integr. \      some, medium            (API + real database)
        /----------\
       /   Unit     \    many, fast, cheap       (pure functions, single modules)
      /--------------\
```

| Layer | Speed | Catches | Run in CI |
|---|---|---|---|
| Static checks (lint, format, types) | seconds | typos, style, obvious bugs | every push |
| Unit | seconds | logic errors | every push |
| Integration | tens of seconds | wiring errors (API ↔ DB) | every push or MR |
| End-to-end | minutes | broken user journeys | against staging, after deploy |
| Smoke | seconds | "is the deployed app alive?" | after every deploy |

**Healthy pipeline principles:** fast feedback first (lint before tests), fail early, keep tests deterministic (no real clocks, random data without seeds, or shared state), and treat a flaky test as a bug.

### 3.2 A richer pipeline: quality, test, and parallel jobs

📄 `.gitlab-ci.yml` (replace your current file)

```yaml
image: node:22-alpine

stages:
  - build
  - quality
  - test
  - security

workflow:
  rules:
    - if: $CI_PIPELINE_SOURCE == "merge_request_event"
    - if: $CI_COMMIT_BRANCH && $CI_OPEN_MERGE_REQUESTS
      when: never
    - if: $CI_COMMIT_BRANCH
    - if: $CI_COMMIT_TAG

variables:
  NPM_CONFIG_CACHE: "$CI_PROJECT_DIR/.npm"

default:
  interruptible: true
  cache:
    key:
      files: [package-lock.json]
    paths: [.npm/]

.node:
  before_script:
    - npm ci --prefer-offline

install:
  extends: .node
  stage: build
  script: [echo "dependencies resolved"]

lint:
  extends: .node
  stage: quality
  needs: [install]
  script: [npm run lint]

format:
  extends: .node
  stage: quality
  needs: [install]
  script: [npm run format:check]

unit-tests:
  extends: .node
  stage: test
  needs: [install]
  parallel:
    matrix:
      - NODE_VERSION: ["20", "22"]
  image: node:${NODE_VERSION}-alpine
  script:
    - npm run test:coverage -- --reporter=default --reporter=junit --outputFile.junit=reports/junit.xml
  coverage: '/All files[^|]*\|[^|]*\s+([\d\.]+)/'
  artifacts:
    when: always
    expire_in: 7 days
    paths: [coverage/]
    reports:
      junit: reports/junit.xml
      coverage_report:
        coverage_format: cobertura
        path: coverage/cobertura-coverage.xml
```

> `parallel: matrix` runs the job once per value, so you verify your code on both Node 20 and Node 22.

### 3.3 Integration tests with a real database (`services`)

A `service` is a sidecar container that starts next to your job. The job reaches it by hostname (the image name, or an `alias`).

💻 Add a Postgres-backed repository so the integration test is meaningful:

```bash
npm install pg
```

📄 `src/db.js`

```js
import pg from "pg";

let pool;

export function getPool() {
  if (!pool) {
    pool = new pg.Pool({ connectionString: process.env.DATABASE_URL });
  }
  return pool;
}

export async function migrate() {
  await getPool().query(`
    CREATE TABLE IF NOT EXISTS books (
      id         uuid PRIMARY KEY DEFAULT gen_random_uuid(),
      title      text NOT NULL,
      author     text NOT NULL,
      created_at timestamptz NOT NULL DEFAULT now()
    )
  `);
}

export async function insertBook({ title, author }) {
  const { rows } = await getPool().query(
    "INSERT INTO books (title, author) VALUES ($1, $2) RETURNING id, title, author",
    [title, author],
  );
  return rows[0];
}

export async function allBooks() {
  const { rows } = await getPool().query("SELECT id, title, author FROM books ORDER BY created_at");
  return rows;
}

export async function closePool() {
  await pool?.end();
  pool = undefined;
}
```

📄 `tests/db.integration.test.js`

```js
import { afterAll, beforeAll, describe, expect, it } from "vitest";
import { allBooks, closePool, insertBook, migrate } from "../src/db.js";

// Runs only when a database is provided (CI sets DATABASE_URL, your laptop may not)
const run = process.env.DATABASE_URL ? describe : describe.skip;

run("database integration", () => {
  beforeAll(async () => {
    await migrate();
  });
  afterAll(async () => {
    await closePool();
  });

  it("stores and reads a book", async () => {
    const saved = await insertBook({ title: "Persuasion", author: "Jane Austen" });
    expect(saved.id).toBeTruthy();
    const all = await allBooks();
    expect(all.some((b) => b.title === "Persuasion")).toBe(true);
  });
});
```

📄 Add this job to `.gitlab-ci.yml`:

```yaml
integration-tests:
  extends: .node
  stage: test
  needs: [install]
  services:
    - name: postgres:16-alpine
      alias: db
  variables:
    POSTGRES_DB: bookshelf_test
    POSTGRES_USER: ci
    POSTGRES_PASSWORD: ci-only-password       # throwaway value for a throwaway container
    DATABASE_URL: "postgres://ci:ci-only-password@db:5432/bookshelf_test"
  script:
    - npm run test -- tests/db.integration.test.js
```

💻 To try the same locally:

```bash
docker run --rm -d --name pg -e POSTGRES_PASSWORD=pw -e POSTGRES_DB=bookshelf_test \
  -p 5432:5432 postgres:16-alpine
DATABASE_URL=postgres://postgres:pw@localhost:5432/bookshelf_test npm run test
docker stop pg
```

> Do not use production credentials, ever, in CI test jobs. Test databases are disposable.

### 3.4 Security scanning in the pipeline ("shift left")

Find problems when they are cheap to fix: while the code is still in a Merge Request.

| Check | What it finds | Tool (free option) |
|---|---|---|
| **Secret detection** | API keys, passwords committed to Git | GitLab `Secret-Detection` template, `gitleaks` |
| **SAST** | insecure code patterns (injection, unsafe functions) | GitLab `SAST` template (Semgrep-based analyzers) |
| **Dependency audit** | vulnerable npm packages | `npm audit`, Renovate/Dependabot |
| **Container scan** | vulnerable OS packages and libraries in your image | Trivy (Level 4) |
| **IaC scan** | misconfigured Terraform/Docker | Trivy config, Checkov |
| **License check** | disallowed licenses | license-checker |

> Some GitLab security features (dashboards, full dependency and container scanning templates, policy enforcement) are tied to paid tiers. The SAST and Secret Detection templates have free-tier versions with fewer features. Check your plan's current feature list. The open-source tools above work on any tier.

📄 Add to `.gitlab-ci.yml`:

```yaml
include:
  - template: Security/SAST.gitlab-ci.yml
  - template: Security/Secret-Detection.gitlab-ci.yml

npm-audit:
  extends: .node
  stage: security
  needs: [install]
  script:
    - npm audit --omit=dev --audit-level=high
  allow_failure: true     # warn, but do not block (decide your own policy in Lab 12)
```

**Policy decisions a real team makes** (write yours down in the lab):

- Which severities **block** a merge (usually Critical, High) and which only warn.
- How long a known vulnerability may stay open (an SLA, for example 7 days for Critical).
- Who may approve an exception, and where it is recorded (an `.trivyignore` with a reason and an expiry date).

### 3.5 Pre-commit hooks as a first line of defence (recap)

CI is the enforcement; hooks give instant feedback.

```bash
npm install -D husky lint-staged @commitlint/cli @commitlint/config-conventional
npx husky init
```

📄 `.husky/pre-commit`

```sh
npx lint-staged
```

📄 `.husky/commit-msg`

```sh
npx --no -- commitlint --edit "$1"
```

📄 `commitlint.config.js`

```js
export default { extends: ["@commitlint/config-conventional"] };
```

📄 add to `package.json`

```json
"lint-staged": {
  "*.js": ["eslint --fix --max-warnings=0", "prettier --write"],
  "*.{json,md,yml,yaml}": ["prettier --write"]
}
```

> In CI set `HUSKY=0` so the `prepare` script does not try to install hooks.

### 3.6 Merge Request quality gates

🌐 **Settings → Merge requests**: enable *Pipelines must succeed*, *All threads must be resolved*, *Squash commits when merging* (as "Encourage" or "Require").
🌐 **Settings → Merge requests → Merge request approvals**: require 1 approval; prevent the author from approving.
🌐 **Settings → Repository → Protected branches**: `main` = no direct push, merge by Maintainers.

> The approval rules and protected environments in later levels depend on your GitLab tier. On the free tier you can still enforce protected branches and *pipelines must succeed*, and you can practice reviewing with a second account.

### 🧠 Knowledge Check: Testing and security

1. Why is a pipeline with only `npm test` not enough for "production quality"?
2. What is a flaky test and what should a team do about it?
3. What does a `service` do in a job and how does the job reach it?
4. Why shouldn't `npm audit` block every build by default?
5. Secret detection found an AWS key in an old commit. List the correct response steps in order.
6. What is "shift left"?
7. Why run unit tests on two Node versions?
8. What is the difference between SAST and dependency scanning?

<details>
<summary>Answers</summary>

1. It misses style/type errors, security problems, integration with real dependencies, build failures, and untested deploy steps.
2. A test that sometimes passes and sometimes fails with no code change. Quarantine it quickly, fix the root cause (timing, shared state, randomness), and track flaky rate; do not just retry.
3. It starts a sidecar container (such as Postgres) for the job's duration. The job connects using the service hostname (image name or `alias`).
4. New advisories appear daily, many do not apply to your usage, and blocking on all of them stops unrelated work. Block only on policy-defined severity and keep an exception process.
5. (1) **Revoke/rotate the key immediately** in AWS. (2) Check CloudTrail for misuse. (3) Remove it from the code and load it from a proper secret store. (4) Optionally rewrite history. (5) Add prevention (pre-commit scanning).
6. Moving checks (tests, security) earlier in the lifecycle, to the developer's machine and the MR, where fixes are cheap.
7. To catch version-specific breakage before an upgrade and to support the versions you claim to support.
8. SAST analyzes *your* source for risky patterns; dependency scanning checks *third-party packages* against known-vulnerability databases.
</details>

### 🛠️ Labs: Level 3

**Lab 10: Quality gates in the MR**

- **Steps:**
  1. Install Husky, lint-staged, and commitlint (section 3.5).
  2. Try `git commit -m "fixed stuff"`. It must fail.
  3. Push using `git push` after a commit made with `--no-verify` that contains a lint error. CI must catch it.
  4. Configure the MR settings in section 3.6.
- ✅ **Done when:** a bad commit message is blocked locally, and a lint error that bypassed hooks is blocked by CI and by "pipelines must succeed".

**Lab 11: Integration tests with Postgres**

- **Steps:**
  1. Add the DB module, the integration test, and the `integration-tests` job.
  2. Run it locally with Docker, then in CI.
  3. Break it by changing the service alias, and read the connection error. Fix it.
  4. Add a second test that verifies an invalid insert (empty title) is rejected by a `CHECK` or `NOT NULL` constraint. Add the migration.
- ✅ **Done when:** the integration job is green in CI and you can explain how `db` resolved to the container.
- 🚀 **Stretch:** Wire the Express routes to use Postgres instead of the in-memory map when `DATABASE_URL` is set.

**Lab 12: Security scanning and policy**

- **Steps:**
  1. Include the SAST and Secret Detection templates and add `npm-audit`.
  2. On a throwaway branch, commit a **fake** secret such as `AKIAIOSFODNN7EXAMPLE` (AWS's documented example key, not a real one) and a deliberately unsafe `eval(req.body.code)`. See what each scanner reports and where.
  3. Write `SECURITY_POLICY.md` in your repo: severities that block, remediation SLAs, exception process.
  4. Add a `.gitleaks.toml` or an allowlist only for the fake test key, with a comment explaining why.
- ✅ **Done when:** at least one scanner flagged your planted issues, you removed them, and your policy file exists.
- ⚠️ Delete the throwaway branch afterwards. Never commit anything that looks real.

**Lab 13: Make the pipeline fast and reliable**

- **Steps:**
  1. Record total pipeline duration and the slowest job.
  2. Apply three optimizations: `needs:`, dependency cache, `interruptible: true`. Optionally run `lint` and `format` in a single job.
  3. Add `timeout: 10 minutes` to every job and `retry` only for `runner_system_failure`.
  4. Record the new duration and the saving.
- ✅ **Done when:** you can show before/after durations and justify every `retry` or `allow_failure` you used.

---

## Level 4: Docker in the pipeline

Docker makes "it works on my machine" mean "it works everywhere", because you ship the machine's relevant parts with the app.

### 4.1 Docker concepts

| Term | Meaning |
|---|---|
| **Image** | An immutable, layered template: OS files + runtime + your app. |
| **Container** | A running instance of an image. |
| **Registry** | A server that stores images (GitLab Container Registry, Docker Hub, AWS ECR). |
| **Tag** | A label on an image version (`app:1.4.2`, `app:3f2a9c1`). Tags are mutable pointers; digests (`@sha256:...`) are immutable. |
| **Volume** | Persistent storage outside the container's disposable filesystem. |
| **Compose** | A YAML file describing several containers that run together. |

```mermaid
flowchart LR
    DF["Dockerfile"] -- "docker build" --> IMG["Image"]
    IMG -- "docker push" --> REG["Registry"]
    REG -- "docker pull" --> HOST["Server"]
    HOST -- "docker run" --> CTR["Container"]
```

### 4.2 Core commands

```bash
docker build -t bookshelf:local .
docker run --rm -p 3000:3000 bookshelf:local
docker run -d --name api -p 3000:3000 -e LOG_LEVEL=debug bookshelf:local
docker ps               # running containers   (docker ps -a: all)
docker logs -f api
docker exec -it api sh  # shell inside the container
docker stop api && docker rm api
docker image ls
docker image prune -f   # remove dangling images
docker system df        # where is my disk going?
docker inspect api      # full JSON details (health, mounts, env)
```

### 4.3 A production-grade Dockerfile

📄 `Dockerfile`

```dockerfile
# syntax=docker/dockerfile:1

# ---- Stage 1: install production dependencies only ----
FROM node:22-alpine AS prod-deps
WORKDIR /app
COPY package.json package-lock.json ./
RUN npm ci --omit=dev --ignore-scripts && npm cache clean --force

# ---- Stage 2: final runtime image ----
FROM node:22-alpine AS runner
WORKDIR /app
ENV NODE_ENV=production

# run as a non-root user
RUN addgroup -S app && adduser -S app -G app

COPY --from=prod-deps --chown=app:app /app/node_modules ./node_modules
COPY --chown=app:app package.json ./
COPY --chown=app:app src ./src

# set at build time so the running app can report which version it is
ARG APP_VERSION=dev
ENV APP_VERSION=$APP_VERSION

USER app
EXPOSE 3000

HEALTHCHECK --interval=30s --timeout=3s --start-period=10s --retries=3 \
  CMD wget -qO- http://127.0.0.1:3000/health || exit 1

CMD ["node", "src/server.js"]
```

📄 `.dockerignore`

```text
node_modules
coverage
reports
.git
.husky
.env
.env.*
Dockerfile
docker-compose*.yml
*.md
tests
```

Why each decision matters:

| Decision | Reason |
|---|---|
| Multi-stage | final image has no dev dependencies or build tools, so it is smaller and has fewer vulnerabilities |
| `alpine` base | small; check native module compatibility before choosing it for every app |
| Copy `package*.json` first | the slow `npm ci` layer is cached until dependencies change |
| Non-root user | a container escape or app exploit does not start with root |
| `HEALTHCHECK` | orchestrators and `docker ps` can see if the app is actually healthy |
| `.dockerignore` | keeps secrets and junk out of the build context and image |
| Exec-form `CMD ["node", ...]` | `node` receives `SIGTERM` directly, so graceful shutdown works |

> For reproducibility, pin base images to a specific version (for example `node:22.11-alpine3.20`), and in high-security setups to a digest. Re-build regularly to pick up OS patches.

### 4.4 Compose for local development

📄 `docker-compose.yml`

```yaml
services:
  api:
    build: .
    ports:
      - "3000:3000"
    environment:
      DATABASE_URL: postgres://bookshelf:devpassword@db:5432/bookshelf
      APP_VERSION: compose-local
    depends_on:
      db:
        condition: service_healthy

  db:
    image: postgres:16-alpine
    environment:
      POSTGRES_USER: bookshelf
      POSTGRES_PASSWORD: devpassword
      POSTGRES_DB: bookshelf
    volumes:
      - pgdata:/var/lib/postgresql/data
    healthcheck:
      test: ["CMD-SHELL", "pg_isready -U bookshelf"]
      interval: 5s
      timeout: 3s
      retries: 10

volumes:
  pgdata:
```

```bash
docker compose up --build -d
docker compose ps
docker compose logs -f api
docker compose down          # add -v to delete the database volume too
```

### 4.5 Build and publish the image in GitLab CI

GitLab gives every project a **Container Registry** (🌐 *Deploy → Container Registry*). CI logs in automatically using predefined variables.

📄 Add to `.gitlab-ci.yml` (add `package` and `scan` to your `stages:` list, after `security`):

```yaml
stages:
  - build
  - quality
  - test
  - security
  - package
  - scan

build-image:
  stage: package
  needs: [unit-tests, lint]
  image: docker:27
  services:
    - docker:27-dind
  variables:
    DOCKER_TLS_CERTDIR: "/certs"
    IMAGE: "$CI_REGISTRY_IMAGE"
  before_script:
    - echo "$CI_REGISTRY_PASSWORD" | docker login -u "$CI_REGISTRY_USER" --password-stdin "$CI_REGISTRY"
  script:
    - docker build --pull --build-arg APP_VERSION="$CI_COMMIT_SHORT_SHA" -t "$IMAGE:$CI_COMMIT_SHORT_SHA" .
    - docker push "$IMAGE:$CI_COMMIT_SHORT_SHA"
    # smoke test: run the image we just built and hit /health
    - docker run -d --name smoke -p 3000:3000 "$IMAGE:$CI_COMMIT_SHORT_SHA"
    - sleep 5
    - docker run --rm --network container:smoke curlimages/curl:8.10.1 -fsS http://localhost:3000/health
    - docker rm -f smoke
  rules:
    - if: $CI_COMMIT_BRANCH =~ /^(test|stage|main)$/
    - if: $CI_COMMIT_TAG

trivy-scan:
  stage: scan
  needs: [build-image]
  image:
    name: aquasec/trivy:0.57.0         # pin a version; update it deliberately
    entrypoint: [""]
  variables:
    TRIVY_USERNAME: "$CI_REGISTRY_USER"
    TRIVY_PASSWORD: "$CI_REGISTRY_PASSWORD"
    TRIVY_NO_PROGRESS: "true"
  script:
    - trivy image --exit-code 0 --severity MEDIUM,HIGH,CRITICAL "$CI_REGISTRY_IMAGE:$CI_COMMIT_SHORT_SHA"
    - trivy image --exit-code 1 --severity CRITICAL --ignore-unfixed "$CI_REGISTRY_IMAGE:$CI_COMMIT_SHORT_SHA"
  rules:
    - if: $CI_COMMIT_BRANCH =~ /^(test|stage|main)$/
    - if: $CI_COMMIT_TAG
```

How it works: the first Trivy run only *reports*; the second *fails the job* on fixable critical vulnerabilities. Tune this policy to your team. The Docker-in-Docker (`dind`) service needs a runner that allows privileged containers; GitLab.com shared Linux runners support it. Rootless alternatives (Buildah, BuildKit) exist if your security team forbids privileged runners.

**Tagging strategy**

| Tag | Mutable? | Purpose |
|---|---|---|
| `:3f2a9c1` (commit SHA) | No | the exact build; **deploy this** |
| `:v1.4.2` (release tag) | No (by convention) | human-readable release |
| `:main`, `:stage` | Yes | "latest of this branch" for convenience |
| `:latest` | Yes | avoid for deployments: you cannot tell what is running |

> Build the image **once** and promote the *same* image through staging and production. Rebuilding per environment means production runs something nobody tested.

### 4.6 Registry hygiene

🌐 **Settings → Packages and registries → Container registry → Cleanup policies**: keep the last N tags and delete tags older than e.g. 30 days, always keeping `v*` release tags. Registries grow forever otherwise.

### 🧠 Knowledge Check: Docker

1. Difference between an image and a container?
2. Why copy `package.json` before the rest of the source in a Dockerfile?
3. Why is `:latest` a poor deployment tag?
4. What is the point of a multi-stage build?
5. Why run the container as a non-root user?
6. What is the difference between `CMD ["node","server.js"]` and `CMD node server.js`?
7. How do you pass configuration to a container without baking it into the image?
8. Why scan an image after building it, and before deploying it?
9. What does `depends_on: condition: service_healthy` add over plain `depends_on`?

<details>
<summary>Answers</summary>

1. An image is the immutable template; a container is a running (or stopped) instance of it.
2. Docker caches layers. Dependencies change rarely, so that expensive layer stays cached while source changes only rebuild later layers.
3. It is mutable, so you cannot tell which build is running, cannot reliably roll back, and caches can serve a stale image.
4. Build with all tools in an early stage, then copy only the artifacts needed to run into a small final image.
5. If the app is compromised, the attacker does not get root inside the container, which limits damage and breakout risk.
6. Exec form runs `node` as PID 1 and it receives signals such as SIGTERM directly; shell form wraps it in `sh -c`, which can swallow signals and break graceful shutdown.
7. Environment variables, mounted secret files, or a secret manager at runtime. Never `COPY .env` into the image.
8. Base images and libraries contain known CVEs; catching a critical one before deploy is cheap, catching it in production is not.
9. It waits until the dependency's health check passes, not merely until its container started.
</details>

### 🛠️ Labs: Level 4

**Lab 14: Containerize and shrink**

- **Steps:**
  1. Write a naive single-stage Dockerfile (`FROM node:22`, `COPY . .`, `npm install`). Build it and record the size with `docker image ls`.
  2. Replace it with the production Dockerfile from 4.3. Record the new size.
  3. Prove layer caching: change one line in `src/app.js` and rebuild; `npm ci` must say `CACHED`.
  4. Verify: `docker exec <id> whoami` prints `app`; `docker inspect` shows `healthy`.
- ✅ **Done when:** you have a size table (before/after) in your log and proof that the container is non-root and healthy.
- 🚀 **Stretch:** Try `node:22-slim` and compare size and vulnerability count to `alpine`.

**Lab 15: Compose stack**

- **Steps:**
  1. Run the Compose file from 4.4 and exercise the API with `curl`.
  2. `docker compose down` then `up` again. Does your data survive? Now run `down -v`. What changed and why?
  3. Add an `adminer` or `pgAdmin` service on port 8080 to inspect the database.
  4. Add a second environment file approach: `docker compose --env-file .env.local up`.
- ✅ **Done when:** you can explain, with evidence, what a volume preserves and what a container loses.

**Lab 16: Build and publish from CI**

- **Steps:**
  1. Add the `build-image` job. Merge to `main` (or push to `test`) and find the image in 🌐 *Deploy → Container Registry*.
  2. Pull and run it on your laptop: `docker login registry.gitlab.com` then `docker run -p 3000:3000 registry.gitlab.com/YOU/bookshelf-api:<sha>`.
  3. Hit `/health` and confirm the reported `version` equals the commit SHA.
  4. Break the smoke test (make `/health` return a 500 on purpose) and see the build fail *before* a bad image is promoted.
- ✅ **Done when:** an image built by CI runs on your laptop and `/health` reports its commit SHA.

**Lab 17: Scan and gate the image**

- **Steps:**
  1. Add `trivy-scan`. Read the report. Count findings by severity.
  2. Switch the Dockerfile base to an old image (`node:16-alpine`) on a throwaway branch and watch the critical gate fail.
  3. Fix by upgrading the base. Record the before/after vulnerability counts.
  4. Add a registry cleanup policy (section 4.6).
- ✅ **Done when:** an outdated base image is blocked by the pipeline and a current one passes.

---

## Level 5: Deploying to a server

Goal: merging code automatically puts a **tested, scanned image** onto a real Linux server, checks that it works, and can **undo itself** if it does not.

```mermaid
flowchart LR
    GL["GitLab CI"] -- "1. push image" --> REG["Registry"]
    GL -- "2. SSH: run deploy.sh" --> SRV["Linux server"]
    SRV -- "3. docker pull" --> REG
    SRV -- "4. compose up -d" --> APP["App container"]
    SRV -- "5. health check" --> APP
    SRV -. "fails: roll back" .-> APP
    USER["Users"] -- "HTTPS" --> CADDY["Caddy proxy"] --> APP
```

### 5.1 Get a server

Pick one:

| Option | Cost | Notes |
|---|---|---|
| **Local VM** (Multipass, VirtualBox) | free | no public URL, but perfect for learning the mechanics |
| **AWS EC2** (Ubuntu 24.04, a small instance) | pay per hour (check your account's free-tier terms) | also teaches security groups and key pairs; stop or terminate when not studying |
| **Any VPS** (DigitalOcean, Hetzner, Linode…) | small monthly fee | simplest public IP |

If you use EC2: 🌐 launch an Ubuntu instance, create a key pair, and set the **Security Group** inbound rules to: SSH (22) from **your IP only**, HTTP (80), HTTPS (443). Never open 22 to `0.0.0.0/0` for a real server.

### 5.2 Prepare and harden the server

💻 SSH in as the default user (`ubuntu` on EC2), then:

```bash
# 1. Updates
sudo apt update && sudo apt -y upgrade

# 2. Dedicated deploy user (no password login, no sudo)
sudo adduser --disabled-password --gecos "" deploy
sudo mkdir -p /home/deploy/.ssh
sudo chmod 700 /home/deploy/.ssh

# 3. Docker
curl -fsSL https://get.docker.com | sudo sh
sudo usermod -aG docker deploy      # note: docker group access is effectively root-equivalent

# 4. Firewall (do NOT enable until SSH is allowed or you lock yourself out)
sudo ufw default deny incoming
sudo ufw default allow outgoing
sudo ufw allow OpenSSH
sudo ufw allow 80/tcp
sudo ufw allow 443/tcp
sudo ufw --force enable

# 5. Brute-force protection and automatic security updates
sudo apt -y install fail2ban unattended-upgrades

# 6. App directory
sudo mkdir -p /opt/bookshelf && sudo chown deploy:deploy /opt/bookshelf
```

Harden SSH: edit `/etc/ssh/sshd_config` (or a file in `/etc/ssh/sshd_config.d/`) and set:

```text
PermitRootLogin no
PasswordAuthentication no
```

```bash
sudo sshd -t && sudo systemctl restart ssh     # test config first; keep your current session open
```

> Open a **second** terminal and confirm you can still log in before closing the first. A bad SSH config is the classic self-lockout.

### 5.3 Create a dedicated CI key pair

💻 On your laptop (a key used *only* by CI, with no passphrase because CI cannot type one):

```bash
ssh-keygen -t ed25519 -f ./ci_deploy_key -C "gitlab-ci-deploy" -N ""
```

- Put `ci_deploy_key.pub` into the server: `/home/deploy/.ssh/authorized_keys` (permissions `600`, owner `deploy`).
- 🌐 In GitLab → **Settings → CI/CD → Variables**, add:

| Key | Type | Value | Flags |
|---|---|---|---|
| `SSH_PRIVATE_KEY` | **File** | contents of `ci_deploy_key` (must end with a newline) | Protected, scope as needed |
| `SSH_KNOWN_HOSTS` | **File** | output of `ssh-keyscan -t ed25519 SERVER_IP` | Protected |
| `SSH_USER` | Variable | `deploy` | |
| `SSH_HOST` | Variable | server IP or DNS name | |

- Then **delete the private key from your laptop** or store it in a password manager. Anyone with it can deploy.

> Optionally restrict the key in `authorized_keys` with `command="/opt/bookshelf/deploy.sh",no-port-forwarding,no-pty ssh-ed25519 AAAA...` so the key can only run the deploy script. This is a nice least-privilege upgrade (requires passing parameters through environment or `SSH_ORIGINAL_COMMAND`).

Let the server pull images: 🌐 GitLab → **Settings → Repository → Deploy tokens** → create a token with `read_registry`, then on the server as `deploy`:

```bash
docker login registry.gitlab.com -u <deploy-token-username> -p <deploy-token>
```

### 5.4 Production compose file and Caddy

On the server, in `/opt/bookshelf`:

📄 `/opt/bookshelf/docker-compose.prod.yml`

```yaml
services:
  api:
    image: ${IMAGE}:${TAG}
    container_name: bookshelf-api
    restart: unless-stopped
    env_file: .env.production
    expose:
      - "3000"
    depends_on:
      db:
        condition: service_healthy
    logging:
      driver: json-file
      options:
        max-size: "10m"
        max-file: "5"
    networks: [internal]

  db:
    image: postgres:16-alpine
    restart: unless-stopped
    env_file: .env.production
    volumes:
      - pgdata:/var/lib/postgresql/data
    healthcheck:
      test: ["CMD-SHELL", "pg_isready -U $${POSTGRES_USER}"]
      interval: 10s
      timeout: 5s
      retries: 5
    networks: [internal]

  caddy:
    image: caddy:2-alpine
    restart: unless-stopped
    ports:
      - "80:80"
      - "443:443"
    volumes:
      - ./Caddyfile:/etc/caddy/Caddyfile:ro
      - caddy_data:/data
    depends_on: [api]
    networks: [internal]

volumes:
  pgdata:
  caddy_data:

networks:
  internal:
```

📄 `/opt/bookshelf/Caddyfile`: **with a domain**, Caddy gets a TLS certificate automatically:

```text
staging.example.com {
    encode gzip
    reverse_proxy api:3000
    header {
        Strict-Transport-Security "max-age=31536000"
        X-Content-Type-Options nosniff
    }
}
```

No domain yet? Use plain HTTP for learning:

```text
:80 {
    reverse_proxy api:3000
}
```

📄 `/opt/bookshelf/.env.production`: **created by hand on the server, never in Git**

```bash
POSTGRES_USER=bookshelf
POSTGRES_PASSWORD=CHANGE_ME_long_random_value
POSTGRES_DB=bookshelf
DATABASE_URL=postgres://bookshelf:CHANGE_ME_long_random_value@db:5432/bookshelf
NODE_ENV=production
LOG_LEVEL=info
```

```bash
chmod 600 /opt/bookshelf/.env.production
```

Generate a strong password with `openssl rand -base64 32`.

### 5.5 The deploy script (with automatic rollback)

📄 `/opt/bookshelf/deploy.sh` (on the server; `chmod +x`)

```bash
#!/usr/bin/env bash
set -euo pipefail

: "${IMAGE:?IMAGE is required}"
: "${TAG:?TAG is required}"

cd /opt/bookshelf
COMPOSE="docker compose -f docker-compose.prod.yml"
STATE=.current_tag
PREVIOUS="$(cat "$STATE" 2>/dev/null || true)"

echo ">> previous: ${PREVIOUS:-none}  new: $TAG"
export IMAGE TAG

docker pull "$IMAGE:$TAG"
$COMPOSE up -d --remove-orphans

echo ">> waiting for container health..."
for i in $(seq 1 30); do
  status="$(docker inspect --format '{{.State.Health.Status}}' bookshelf-api 2>/dev/null || echo none)"
  if [ "$status" = "healthy" ]; then
    echo ">> healthy"
    echo "$TAG" > "$STATE"
    docker image prune -f >/dev/null
    exit 0
  fi
  sleep 2
done

echo "!! new version did not become healthy"
docker logs --tail 50 bookshelf-api || true

if [ -n "$PREVIOUS" ]; then
  echo "!! rolling back to $PREVIOUS"
  export TAG="$PREVIOUS"
  $COMPOSE up -d
fi
exit 1
```

### 5.6 CI/CD deploy jobs with environments

📄 Add `deploy` and `verify` to `stages:` (after `scan`), then add the jobs:

```yaml
.deploy:
  stage: deploy
  image: alpine:3.20
  before_script:
    - apk add --no-cache openssh-client curl
    - mkdir -p ~/.ssh && chmod 700 ~/.ssh
    - cp "$SSH_PRIVATE_KEY" ~/.ssh/id_ed25519 && chmod 600 ~/.ssh/id_ed25519
    - cp "$SSH_KNOWN_HOSTS" ~/.ssh/known_hosts
  script:
    - ssh "$SSH_USER@$SSH_HOST" "IMAGE=$CI_REGISTRY_IMAGE TAG=$CI_COMMIT_SHORT_SHA /opt/bookshelf/deploy.sh"

deploy-staging:
  extends: .deploy
  needs: [build-image, trivy-scan]
  resource_group: staging              # never two deploys at once
  environment:
    name: staging
    url: http://$SSH_HOST              # or https://staging.example.com
  rules:
    - if: $CI_COMMIT_BRANCH == "stage"

smoke-staging:
  stage: verify
  image: curlimages/curl:8.10.1
  needs: [deploy-staging]
  script:
    - curl -fsS --retry 5 --retry-delay 3 "http://$SSH_HOST/health"
  rules:
    - if: $CI_COMMIT_BRANCH == "stage"

deploy-production:
  extends: .deploy
  needs: [build-image, trivy-scan]
  resource_group: production
  environment:
    name: production
    url: http://$SSH_HOST
  rules:
    - if: $CI_COMMIT_BRANCH == "main"
      when: manual                     # a human approves production
```

Use **different** `SSH_HOST` / key variable values for staging and production by giving each variable an **environment scope** (🌐 Variables → *Environment scope* = `staging` or `production`). Environment-scoped variables are available only to jobs whose `environment:name` matches.

🌐 **Settings → CI/CD → Protected environments** (a paid-tier feature on some plans): restrict who may deploy to `production`. On free plans, rely on protected branches plus manual jobs.

### 5.7 Rollback

Because every image is tagged with its commit SHA, rollback = deploy an older tag.

```bash
ssh deploy@SERVER
cat /opt/bookshelf/.current_tag                # what is running now
IMAGE=registry.gitlab.com/YOU/bookshelf-api TAG=<older-sha> /opt/bookshelf/deploy.sh
```

Or in GitLab: 🌐 **Operate → Environments → production → Rollback** on an earlier deployment (re-runs that deploy job).

> **Databases complicate rollback.** Code can go back; data cannot. Use backward-compatible migrations (expand → migrate → contract): add a column in release N, switch code in N+1, remove the old column in N+2.

### 5.8 Operating the server

```bash
docker compose -f /opt/bookshelf/docker-compose.prod.yml ps
docker compose -f /opt/bookshelf/docker-compose.prod.yml logs -f --tail 100 api
docker stats --no-stream
df -h && free -m
sudo ufw status verbose
sudo journalctl -u docker -n 100 --no-pager
sudo fail2ban-client status sshd
```

Nightly database backup (cron as `deploy`: `crontab -e`):

```text
0 2 * * * docker exec $(docker ps -qf name=db) pg_dump -U bookshelf bookshelf | gzip > /opt/bookshelf/backups/db-$(date +\%F).sql.gz
```

Copy backups **off the server** (S3, in Level 6). A backup you have never restored is a hope, not a backup.

### 🧠 Knowledge Check: Deployment

1. Why use a dedicated `deploy` user and a CI-only SSH key?
2. What does `resource_group` prevent?
3. Why deploy by commit-SHA tag rather than `:latest`?
4. Why must `.env.production` not be in Git or in the image?
5. What does the deploy script do if the new container never becomes healthy?
6. Why is "expand → migrate → contract" safer for database changes?
7. How do you limit the damage if the CI SSH key leaks?
8. Why is `staging` supposed to mirror production?

<details>
<summary>Answers</summary>

1. Least privilege and revocability: if the key leaks, you revoke one key for one limited account, not your personal access.
2. Two deployments to the same environment running at the same time (race conditions, half-applied releases).
3. A SHA is immutable and traceable; `latest` moves, so you cannot tell what runs or reliably roll back.
4. Secrets in Git or image layers can be read by anyone with access and persist in history; they must be injected at runtime.
5. Prints container logs, redeploys the previously recorded tag, and exits non-zero so the pipeline is marked failed.
6. Old and new code can both run against the schema during rollout and rollback, so you are never forced to restore data.
7. Remove it from `authorized_keys`, restrict it to a forced command, scope it to the deploy host only, firewall SSH to known IPs, and rotate regularly.
8. Problems that only appear with production-like config (versions, proxy, TLS, data shape) show up before real users see them.
</details>

### 🛠️ Labs: Level 5

**Lab 18: Provision and harden a server**

- **Steps:** Follow 5.1 to 5.3. Verify from a second terminal: password login is refused, root login is refused, `ufw status` lists only 22/80/443, and `fail2ban-client status sshd` works.
- ✅ **Done when:** `ssh -o PasswordAuthentication=yes deploy@SERVER` is rejected and only key login works.
- ⚠️ If on AWS, note the instance ID in `CLEANUP.md` and stop the instance after studying.

**Lab 19: Deploy to staging from the pipeline**

- **Steps:**
  1. Create the files in 5.4 and 5.5 on the server.
  2. Add the CI variables (5.3) and the deploy jobs (5.6).
  3. Merge a change into `stage`. Watch the deploy job, then `curl http://SERVER/health` yourself.
  4. Change the response of `/health` (add a field), merge again, and confirm the new `version` appears.
- ✅ **Done when:** merging into `stage` changes the live app with no manual steps, and the GitLab **Environments** page lists the deployment.

**Lab 20: Break it and roll back**

- **Steps:**
  1. Merge a change that makes the app crash at startup (`throw new Error("boom")` in `server.js`).
  2. Watch `deploy.sh` detect the unhealthy container and roll back. Confirm the old version still answers on `/health`.
  3. Fix the bug and redeploy.
  4. Practice a **manual** rollback to a known-good SHA using section 5.7 and time it. Target: under 2 minutes.
  5. Set up the nightly backup, restore it into a scratch database, and verify a row.
- ✅ **Done when:** the bad release never served traffic for more than a few seconds, and you restored a backup successfully.
- 🚀 **Stretch:** Make the deploy job post a message to Slack or email on success and on failure.

---

## Level 6: AWS

You will use AWS the way a real team does: **no long-lived keys in CI**, images in **ECR**, the app on **ECS Fargate** behind a **load balancer**, with **logs, alarms, and secrets** handled properly, and everything cleaned up afterwards.

### 6.1 Core AWS concepts for CI/CD

| Service | What it is | Used for |
|---|---|---|
| **IAM** | users, roles, policies | who and what may do what |
| **STS** | issues temporary credentials | CI assumes a role for a few minutes |
| **ECR** | private Docker registry | store your images |
| **ECS (Fargate)** | run containers without managing servers | run the app |
| **ALB** | application load balancer | public entry, health checks, spreads traffic |
| **VPC / subnets / security groups** | virtual network and firewalls | network isolation |
| **CloudWatch** | logs, metrics, alarms | observability |
| **Secrets Manager / SSM Parameter Store** | encrypted config | database passwords, API keys |
| **S3** | object storage | backups, static sites, artifacts |
| **CloudFront** | CDN | fast, HTTPS static delivery |
| **Route 53 / ACM** | DNS / certificates | your domain and TLS |

**IAM rules to live by**

1. Never use root. Never create access keys for humans or CI if a role can do it.
2. **Least privilege:** grant only the actions and resources needed.
3. Prefer **roles with temporary credentials** over IAM users with static keys.
4. Everything is identified by an **ARN**: `arn:aws:service:region:account-id:resource`.

### 6.2 Install and use the AWS CLI

```bash
aws --version
aws configure sso            # recommended for humans (IAM Identity Center)
# or, for a learning IAM user: aws configure
aws sts get-caller-identity  # "who am I?" You will run this a lot.
export AWS_REGION=ap-south-1
```

### 6.3 CI authenticates to AWS with OIDC (no stored keys)

GitLab can issue a short-lived signed token (JWT) for each job. AWS verifies it and hands the job temporary credentials, **only if** the token matches the project and branch you trust.

```mermaid
sequenceDiagram
    participant Job as GitLab job
    participant GL as GitLab (OIDC issuer)
    participant STS as AWS STS
    participant AWS as ECR / ECS
    Job->>GL: request ID token (aud=https://gitlab.com)
    GL-->>Job: signed JWT (project, branch, pipeline)
    Job->>STS: AssumeRoleWithWebIdentity(role, JWT)
    STS->>STS: verify signature + trust-policy conditions
    STS-->>Job: temporary credentials (about 1 hour)
    Job->>AWS: calls using temporary credentials
```

**Step 1: 🌐 Create the identity provider.** IAM → **Identity providers → Add provider → OpenID Connect**. Provider URL: `https://gitlab.com`. Audience: `https://gitlab.com`. (Self-managed GitLab: use your own URL.)

**Step 2: 🌐 Create the role** `gitlab-ci-bookshelf` with this **trust policy** (replace the account ID and project path):

```json
{
  "Version": "2012-10-17",
  "Statement": [
    {
      "Effect": "Allow",
      "Principal": {
        "Federated": "arn:aws:iam::123456789012:oidc-provider/gitlab.com"
      },
      "Action": "sts:AssumeRoleWithWebIdentity",
      "Condition": {
        "StringEquals": { "gitlab.com:aud": "https://gitlab.com" },
        "StringLike": {
          "gitlab.com:sub": "project_path:YOUR_GROUP/bookshelf-api:ref_type:branch:ref:main"
        }
      }
    }
  ]
}
```

The `sub` condition is the security heart of this setup. It says: *only jobs from this project, on the `main` branch, may assume this role.* A weaker pattern such as `project_path:*` would let any project on gitlab.com assume your role. Use one role per environment (for example staging role: branch `stage`; production role: branch `main`).

**Step 3: attach a permissions policy** (least privilege; start with ECR push, add ECS later):

```json
{
  "Version": "2012-10-17",
  "Statement": [
    {
      "Effect": "Allow",
      "Action": "ecr:GetAuthorizationToken",
      "Resource": "*"
    },
    {
      "Effect": "Allow",
      "Action": [
        "ecr:BatchCheckLayerAvailability",
        "ecr:InitiateLayerUpload",
        "ecr:UploadLayerPart",
        "ecr:CompleteLayerUpload",
        "ecr:PutImage",
        "ecr:BatchGetImage",
        "ecr:GetDownloadUrlForLayer"
      ],
      "Resource": "arn:aws:ecr:ap-south-1:123456789012:repository/bookshelf-api"
    }
  ]
}
```

**Step 4: CI job.** Add `AWS_ROLE_ARN`, `AWS_REGION`, `AWS_ACCOUNT_ID` as CI variables (not secret, but scope them), then:

```yaml
.aws-auth:
  id_tokens:
    GITLAB_OIDC_TOKEN:
      aud: https://gitlab.com
  before_script:
    - apk add --no-cache aws-cli jq
    - echo "$GITLAB_OIDC_TOKEN" > /tmp/oidc-token
    - export AWS_WEB_IDENTITY_TOKEN_FILE=/tmp/oidc-token
    - export AWS_ROLE_ARN="$AWS_ROLE_ARN"
    - export AWS_ROLE_SESSION_NAME="gitlab-$CI_PROJECT_ID-$CI_PIPELINE_ID"
    - aws sts get-caller-identity      # proves who the job is, prints no secrets
```

Test it with a tiny job first:

```yaml
aws-whoami:
  extends: .aws-auth
  stage: build
  image: alpine:3.20
  script:
    - echo "authenticated as above"
  rules:
    - if: $CI_COMMIT_BRANCH == "main"
```

### 6.4 ECR: push your image to AWS

💻 Create the repository (once):

```bash
aws ecr create-repository --repository-name bookshelf-api \
  --image-scanning-configuration scanOnPush=true \
  --image-tag-mutability IMMUTABLE \
  --region $AWS_REGION
```

`IMMUTABLE` tags mean a tag like `3f2a9c1` can never be overwritten, which is exactly what you want for traceable releases.

Lifecycle policy to control cost (keep only the 20 newest images): 🌐 ECR → repository → **Lifecycle policies**, or:

```bash
aws ecr put-lifecycle-policy --repository-name bookshelf-api --lifecycle-policy-text '{
  "rules": [{
    "rulePriority": 1,
    "description": "keep last 20 images",
    "selection": { "tagStatus": "any", "countType": "imageCountMoreThan", "countNumber": 20 },
    "action": { "type": "expire" }
  }]
}'
```

📄 CI job to build and push to ECR:

```yaml
push-ecr:
  extends: .aws-auth
  stage: package
  image: docker:27
  services: ["docker:27-dind"]
  needs: [unit-tests, lint]
  variables:
    DOCKER_TLS_CERTDIR: "/certs"
    ECR: "$AWS_ACCOUNT_ID.dkr.ecr.$AWS_REGION.amazonaws.com"
  script:
    - aws ecr get-login-password --region "$AWS_REGION" | docker login --username AWS --password-stdin "$ECR"
    - docker build --build-arg APP_VERSION="$CI_COMMIT_SHORT_SHA" -t "$ECR/bookshelf-api:$CI_COMMIT_SHORT_SHA" .
    - docker push "$ECR/bookshelf-api:$CI_COMMIT_SHORT_SHA"
  rules:
    - if: $CI_COMMIT_BRANCH == "main"
```

### 6.5 Run on ECS Fargate behind a load balancer

```mermaid
flowchart TB
    U["Users"] --> ALB["Application Load Balancer<br/>(public subnets)"]
    ALB --> T1["Fargate task"] 
    ALB --> T2["Fargate task"]
    T1 --> CW["CloudWatch Logs"]
    T2 --> CW
    T1 --> SM["Secrets Manager"]
    ECR["ECR image"] -.-> T1
    ECR -.-> T2
```

⚠️ **Cost guardrails before you create anything:**

- An **ALB** bills by the hour while it exists, even with no traffic.
- **Fargate tasks** bill per vCPU-second and GB-second while running.
- A **NAT Gateway** bills hourly plus per-GB; for learning, avoid it by running tasks in public subnets with a public IP (and tight security groups). Real production usually uses private subnets + NAT or VPC endpoints.
- Record every resource in `CLEANUP.md` and **delete everything at the end of the session** (Lab 23 includes teardown). Check current prices on the AWS pricing pages.

**Console walkthrough (first time):**

1. 🌐 **ECS → Clusters → Create cluster** → name `bookshelf`, infrastructure **AWS Fargate**.
2. 🌐 **CloudWatch → Log groups → Create** `/ecs/bookshelf-api`, retention 7 days (cost control).
3. 🌐 **IAM → Roles → Create role**: trusted entity *Elastic Container Service Task*, attach `AmazonECSTaskExecutionRolePolicy`, name `ecsTaskExecutionRole` (it lets ECS pull from ECR and write logs). Later add permission to read your secret.
4. 🌐 **EC2 → Security groups**: `alb-sg` (inbound 80 from anywhere) and `task-sg` (inbound 3000 **only from `alb-sg`**).
5. 🌐 **EC2 → Target groups**: type *IP*, protocol HTTP, port 3000, health check path `/health`.
6. 🌐 **EC2 → Load balancers → Create ALB** (internet-facing, 2 public subnets, `alb-sg`, listener 80 → the target group).
7. 🌐 **ECS → Task definitions → Create** (Fargate, 0.25 vCPU, 0.5 GB), container `api`, image `ACCOUNT.dkr.ecr.REGION.amazonaws.com/bookshelf-api:SHA`, port 3000, log driver `awslogs` with group `/ecs/bookshelf-api`.
8. 🌐 **ECS → Cluster → Create service**: launch type Fargate, desired tasks 2, attach the ALB and target group, security group `task-sg`, public IP enabled (learning only), and **turn on the deployment circuit breaker with rollback**.

Then test: `curl http://<ALB-DNS-NAME>/health`.

The **deployment circuit breaker** makes ECS automatically roll back a deployment whose new tasks keep failing to become healthy, which is the same safety net as `deploy.sh`, provided by the platform.

**Deploy from CI:** register a new task-definition revision with the new image, update the service, and wait for it to become stable.

📄 `scripts/ecs-deploy.sh`

```bash
#!/usr/bin/env bash
set -euo pipefail

: "${AWS_REGION:?}" "${CLUSTER:?}" "${SERVICE:?}" "${TASK_FAMILY:?}" "${IMAGE_URI:?}"

echo ">> current task definition: $TASK_FAMILY"
aws ecs describe-task-definition --task-definition "$TASK_FAMILY" \
  --query taskDefinition > /tmp/taskdef.json

# swap in the new image and drop fields that register-task-definition rejects
jq --arg IMAGE "$IMAGE_URI" '
  .containerDefinitions[0].image = $IMAGE
  | del(.taskDefinitionArn, .revision, .status, .requiresAttributes,
        .compatibilities, .registeredAt, .registeredBy)
' /tmp/taskdef.json > /tmp/new-taskdef.json

NEW_ARN=$(aws ecs register-task-definition --cli-input-json file:///tmp/new-taskdef.json \
  --query 'taskDefinition.taskDefinitionArn' --output text)
echo ">> registered: $NEW_ARN"

aws ecs update-service --cluster "$CLUSTER" --service "$SERVICE" --task-definition "$NEW_ARN" >/dev/null
echo ">> waiting for service to become stable (circuit breaker will roll back on failure)..."
aws ecs wait services-stable --cluster "$CLUSTER" --services "$SERVICE"
echo ">> deployed $IMAGE_URI"
```

📄 CI job:

```yaml
deploy-ecs:
  extends: .aws-auth
  stage: deploy
  image: alpine:3.20
  needs: [push-ecr]
  resource_group: production
  variables:
    CLUSTER: bookshelf
    SERVICE: bookshelf-api
    TASK_FAMILY: bookshelf-api
  environment:
    name: production
  script:
    - apk add --no-cache bash
    - export IMAGE_URI="$AWS_ACCOUNT_ID.dkr.ecr.$AWS_REGION.amazonaws.com/bookshelf-api:$CI_COMMIT_SHORT_SHA"
    - bash scripts/ecs-deploy.sh
  rules:
    - if: $CI_COMMIT_BRANCH == "main"
      when: manual
```

Add to the CI role's permission policy: `ecs:DescribeTaskDefinition`, `ecs:RegisterTaskDefinition`, `ecs:UpdateService`, `ecs:DescribeServices`, and `iam:PassRole` **restricted** to the task execution and task roles (and only to ECS via `iam:PassedToService`). `PassRole` is a classic privilege-escalation path; never grant it on `*`.

### 6.6 Secrets and configuration

| Where | Use for | Notes |
|---|---|---|
| **SSM Parameter Store** | plain or encrypted (SecureString) config | simple, inexpensive |
| **Secrets Manager** | passwords and keys that need rotation | a per-secret monthly fee applies |
| **Task definition `environment`** | non-secret config | visible in the console |
| **Task definition `secrets`** | secret references | injected at start; the execution role needs read access to that one secret |

```json
"secrets": [
  { "name": "DATABASE_URL",
    "valueFrom": "arn:aws:secretsmanager:ap-south-1:123456789012:secret:bookshelf/db-AbCdEf" }
]
```

### 6.7 Observability: logs and alarms

```bash
# tail logs from your terminal
aws logs tail /ecs/bookshelf-api --follow

# a simple alarm: more than 5 target 5xx responses in 5 minutes
aws cloudwatch put-metric-alarm \
  --alarm-name bookshelf-5xx \
  --namespace AWS/ApplicationELB --metric-name HTTPCode_Target_5XX_Count \
  --dimensions Name=LoadBalancer,Value=app/YOUR-ALB/ID \
  --statistic Sum --period 300 --evaluation-periods 1 \
  --threshold 5 --comparison-operator GreaterThanThreshold \
  --alarm-actions arn:aws:sns:ap-south-1:123456789012:bookshelf-alerts
```

Create an **SNS topic** `bookshelf-alerts` and subscribe your email to receive it. Also create alarms for: unhealthy host count, CPU/memory above 80%, and a **billing** alarm.

### 6.8 S3 for backups and static sites

```bash
aws s3 mb s3://bookshelf-backups-YOUR-UNIQUE-NAME
aws s3api put-public-access-block --bucket bookshelf-backups-YOUR-UNIQUE-NAME \
  --public-access-block-configuration BlockPublicAcls=true,IgnorePublicAcls=true,BlockPublicPolicy=true,RestrictPublicBuckets=true
aws s3api put-bucket-versioning --bucket bookshelf-backups-YOUR-UNIQUE-NAME \
  --versioning-configuration Status=Enabled
aws s3 cp backup.sql.gz s3://bookshelf-backups-YOUR-UNIQUE-NAME/db/
```

Rules: block public access by default, enable versioning for important data, add a lifecycle rule to expire old backups, and encrypt at rest (default on for new buckets).

### 🧠 Knowledge Check: AWS

1. Why is OIDC better than storing `AWS_ACCESS_KEY_ID` in CI variables?
2. What would go wrong with a trust-policy `sub` of `project_path:*`?
3. Why make ECR tags immutable?
4. What does the ECS deployment circuit breaker do?
5. Why should the task security group allow port 3000 only from the ALB security group?
6. What is the difference between the task execution role and the task role?
7. Why is `iam:PassRole` on `*` dangerous?
8. Name four AWS resources that commonly cause surprise bills.
9. Where would you put a database password for an ECS task, and why not in the image?

<details>
<summary>Answers</summary>

1. No long-lived secret to leak or rotate; credentials are temporary and tied to a specific project and branch.
2. Any project on gitlab.com could request a token and assume your role.
3. A tag can never be overwritten, so what you scanned, tested, and deployed is guaranteed to be the same bytes.
4. If new tasks fail to reach healthy state, ECS stops the rollout and automatically goes back to the last good version.
5. Users can only reach the app through the load balancer, which shrinks the attack surface.
6. The execution role is used by ECS itself (pull images, write logs, fetch secrets); the task role is what your application code uses to call AWS APIs.
7. It lets a principal hand any role (including powerful ones) to a service, which can lead to privilege escalation.
8. NAT Gateways, Application Load Balancers, running EC2/Fargate/RDS you forgot, unattached Elastic IPs, unexpectedly large CloudWatch logs or data transfer.
9. Secrets Manager or SSM SecureString, referenced via the task definition `secrets` field. Images are readable by anyone with pull access and persist in layers.
</details>

### 🛠️ Labs: Level 6

**Lab 21: AWS safety and keyless CI**

- **Steps:**
  1. Complete the safety checklist in section 0.2 (MFA, budget alarm, non-root user).
  2. Create the OIDC provider and role from 6.3, scoped to `main` of your project.
  3. Run the `aws-whoami` job on `main`; confirm the printed ARN contains your role.
  4. Run the same job on a feature branch. It must be **denied**. Capture the error message.
- ✅ **Done when:** `main` can assume the role, a feature branch cannot, and **no AWS keys exist anywhere in GitLab variables**.

**Lab 22: Push to ECR from CI**

- **Steps:**
  1. Create the ECR repo with immutable tags and a lifecycle policy.
  2. Add the least-privilege policy and the `push-ecr` job.
  3. Try pushing the same tag twice. Read the immutability error.
  4. Open the ECR console and read the scan-on-push findings.
- ✅ **Done when:** an image tagged with your commit SHA is in ECR and a duplicate push is rejected.

**Lab 23: Deploy to ECS Fargate and roll back**

- **Steps:**
  1. Build the ECS stack (6.5). Record each resource in `CLEANUP.md`.
  2. Reach the app via the ALB DNS name.
  3. Deploy a new version through the pipeline and watch `ecs wait services-stable`.
  4. Deploy a **broken** version (crash at startup). Observe the circuit breaker roll back. Check the **Events** tab of the service.
  5. **Tear down:** delete the service (scale to 0 first), cluster, ALB, target group, security groups, log group, and ECR images you do not need.
- ✅ **Done when:** a good deploy succeeds, a bad deploy rolls back automatically, and `CLEANUP.md` shows every resource deleted (confirm in the Billing console that nothing is still accruing).
- ⚠️ Do the teardown the same day.

**Lab 24: Secrets and alarms**

- **Steps:**
  1. Store the database URL in Secrets Manager (or an SSM SecureString) and reference it from the task definition.
  2. Grant the execution role read access to *that one secret only*.
  3. Confirm the app reads it and that the value does not appear in the console task definition.
  4. Create the SNS topic, subscribe your email, and create the 5xx alarm. Trigger it by forcing 500s and wait for the email.
  5. Upload a database backup to S3 with public access blocked and versioning on.
- ✅ **Done when:** you receive the alarm email, and you can show the secret is referenced, not embedded.

---

## Level 7: Advanced CI/CD

At this level you stop writing pipelines for one repo and start designing **delivery systems**: reusable, secure, measurable, and recoverable.

### 7.1 Pipeline architecture

#### Reusable templates

Put shared jobs in one repository (`platform/ci-templates`) and version them with tags, so teams get fixes without copy-paste.

📄 `platform/ci-templates` → `node.yml`

```yaml
.node-base:
  image: node:22-alpine
  variables:
    NPM_CONFIG_CACHE: "$CI_PROJECT_DIR/.npm"
  cache:
    key:
      files: [package-lock.json]
    paths: [.npm/]
  before_script:
    - npm ci --prefer-offline

.node-lint:
  extends: .node-base
  stage: quality
  script: [npm run lint]
```

📄 Consumer project `.gitlab-ci.yml`

```yaml
include:
  - project: "platform/ci-templates"
    ref: v1.3.0                    # pin a tag, never a moving branch, in production
    file: "/node.yml"

lint:
  extends: .node-lint
```

GitLab also offers **CI/CD components** (versioned, parameterized building blocks published in a catalog). Look at the current documentation for the syntax your GitLab version supports.

#### Parent-child pipelines (monorepos, big systems)

```yaml
# parent: .gitlab-ci.yml
stages: [trigger]

api-pipeline:
  stage: trigger
  trigger:
    include: services/api/.gitlab-ci.yml
    strategy: depend            # parent fails if child fails
  rules:
    - changes: ["services/api/**/*"]

web-pipeline:
  stage: trigger
  trigger:
    include: services/web/.gitlab-ci.yml
    strategy: depend
  rules:
    - changes: ["services/web/**/*"]
```

Only the pipelines for the code that changed run, which saves time and money in a monorepo.

#### Scheduled and API-triggered pipelines

🌐 **Build → Pipeline schedules**: run nightly to catch dependency drift, run slow end-to-end suites, and rebuild images so OS security patches arrive. Use `rules: - if: $CI_PIPELINE_SOURCE == "schedule"` for jobs that should only run on schedules.

### 7.2 Review apps (a temporary environment per Merge Request)

Reviewers click a link and test the actual change.

📄 Add to `.gitlab-ci.yml`:

```yaml
review-deploy:
  extends: .deploy-base-for-review     # your SSH setup from Level 5
  stage: deploy
  environment:
    name: review/$CI_COMMIT_REF_SLUG
    url: http://$SSH_HOST:$REVIEW_PORT
    on_stop: review-stop
    auto_stop_in: 2 days               # cleans itself up
  script:
    - export REVIEW_PORT=$((4000 + CI_MERGE_REQUEST_IID))
    - ssh "$SSH_USER@$SSH_HOST" "docker rm -f review-$CI_MERGE_REQUEST_IID 2>/dev/null; docker run -d --name review-$CI_MERGE_REQUEST_IID -p $REVIEW_PORT:3000 --memory 256m $CI_REGISTRY_IMAGE:$CI_COMMIT_SHORT_SHA"
  rules:
    - if: $CI_PIPELINE_SOURCE == "merge_request_event"

review-stop:
  stage: deploy
  environment:
    name: review/$CI_COMMIT_REF_SLUG
    action: stop
  script:
    - ssh "$SSH_USER@$SSH_HOST" "docker rm -f review-$CI_MERGE_REQUEST_IID"
  rules:
    - if: $CI_PIPELINE_SOURCE == "merge_request_event"
      when: manual
```

> ⚠️ Review apps run **unreviewed code** on real infrastructure. Use a separate, isolated host, no production secrets or data, resource limits, and only for trusted contributors. Never run untrusted fork code with access to your credentials.

### 7.3 Infrastructure as Code with Terraform

Clicking in the console does not scale and cannot be reviewed. Terraform describes infrastructure as code, so changes go through the same MR → pipeline → approval flow as application code.

```mermaid
flowchart LR
    MR["MR with .tf changes"] --> PLAN["terraform plan<br/>(read-only role)"]
    PLAN --> REVIEW["Human reviews the plan"]
    REVIEW --> MERGE["Merge to main"]
    MERGE --> APPLY["terraform apply<br/>(manual, privileged role)"]
    APPLY --> STATE[("Remote state in S3 + locking")]
```

📄 `infra/main.tf`

```hcl
terraform {
  required_version = ">= 1.9"

  required_providers {
    aws = {
      source  = "hashicorp/aws"
      version = "~> 6.0"     # pin the major version you tested; check the registry for the current one
    }
  }

  backend "s3" {
    bucket  = "YOUR-UNIQUE-tfstate-bucket"   # create once by hand, with versioning and encryption
    key     = "bookshelf/terraform.tfstate"
    region  = "ap-south-1"
    encrypt = true
    # State locking prevents two applies at once. Use a DynamoDB table
    # (dynamodb_table = "tf-locks") or, on recent Terraform versions, use_lockfile = true.
    use_lockfile = true
  }
}

provider "aws" {
  region = var.region
}

variable "region" {
  type    = string
  default = "ap-south-1"
}

resource "aws_ecr_repository" "app" {
  name                 = "bookshelf-api"
  image_tag_mutability = "IMMUTABLE"

  image_scanning_configuration {
    scan_on_push = true
  }
}

resource "aws_ecr_lifecycle_policy" "app" {
  repository = aws_ecr_repository.app.name
  policy = jsonencode({
    rules = [{
      rulePriority = 1
      description  = "keep last 20 images"
      selection    = { tagStatus = "any", countType = "imageCountMoreThan", countNumber = 20 }
      action       = { type = "expire" }
    }]
  })
}

resource "aws_cloudwatch_log_group" "app" {
  name              = "/ecs/bookshelf-api"
  retention_in_days = 7
}

output "ecr_repository_url" {
  value = aws_ecr_repository.app.repository_url
}
```

📄 CI jobs

```yaml
.terraform:
  image:
    name: hashicorp/terraform:1.9
    entrypoint: [""]
  id_tokens:
    GITLAB_OIDC_TOKEN:
      aud: https://gitlab.com
  before_script:
    - echo "$GITLAB_OIDC_TOKEN" > /tmp/oidc-token
    - export AWS_WEB_IDENTITY_TOKEN_FILE=/tmp/oidc-token
    - cd infra
    - terraform init -input=false

tf-validate:
  extends: .terraform
  stage: quality
  variables:
    AWS_ROLE_ARN: "$TF_PLAN_ROLE_ARN"        # read-only role, trusted for MR branches
  script:
    - terraform fmt -check -recursive
    - terraform validate

tf-plan:
  extends: .terraform
  stage: test
  variables:
    AWS_ROLE_ARN: "$TF_PLAN_ROLE_ARN"
  script:
    - terraform plan -input=false -out=tfplan
    - terraform show -no-color tfplan > plan.txt
  artifacts:
    expire_in: 1 day                          # plan files can contain sensitive values
    paths: [infra/tfplan, infra/plan.txt]

tf-apply:
  extends: .terraform
  stage: deploy
  needs: [tf-plan]
  resource_group: terraform
  variables:
    AWS_ROLE_ARN: "$TF_APPLY_ROLE_ARN"       # privileged role, trusted for main only
  script:
    - terraform apply -input=false tfplan
  rules:
    - if: $CI_COMMIT_BRANCH == "main"
      when: manual
```

Use **two roles**: a read-only *plan* role (trusted for any branch of your project, so MRs can plan) and a write *apply* role trusted for **`main` only**.

> Never commit `*.tfstate`, `*.tfvars` containing secrets, or the `.terraform/` directory. State can contain secrets: encrypt the bucket, restrict access, and keep versioning on.

### 7.4 Deployment strategies

| Strategy | How it works | Downtime | Rollback speed | Cost / complexity |
|---|---|---|---|---|
| **Recreate** | stop old, start new | brief | redeploy old | lowest |
| **Rolling** | replace instances gradually (ECS default) | none | redeploy old, or automatic | low |
| **Blue/Green** | run the new version beside the old, switch traffic, keep old for instant rollback | none | **instant** (switch back) | double capacity during deploy |
| **Canary** | send a small share of traffic (for example 5%) to the new version, watch metrics, then increase | none | fast (set share to 0) | needs traffic splitting + good metrics |
| **Feature flags** | deploy code dark, enable per user/percentage at runtime | none | instant (flip flag) | needs flag system and discipline |

Key idea: **deploy ≠ release.** Deployment puts code on servers; release exposes it to users. Feature flags and canaries separate the two, which is how mature teams ship many times a day safely.

📄 Teaching sketch: blue/green on one server (`/opt/bookshelf/bluegreen.sh`). Set `container_name: caddy` for the proxy in compose and keep `Caddyfile.tmpl` with `reverse_proxy api-blue:3000`.

```bash
#!/usr/bin/env bash
set -euo pipefail
: "${IMAGE:?}" "${TAG:?}"
cd /opt/bookshelf

ACTIVE="$(cat .active_color 2>/dev/null || echo blue)"
if [ "$ACTIVE" = "blue" ]; then NEXT=green; else NEXT=blue; fi
echo ">> active=$ACTIVE next=$NEXT"

docker pull "$IMAGE:$TAG"
docker rm -f "api-$NEXT" 2>/dev/null || true
docker run -d --name "api-$NEXT" --network bookshelf_internal \
  --env-file .env.production \
  --health-cmd "wget -qO- http://127.0.0.1:3000/health || exit 1" \
  --health-interval 5s --health-retries 3 \
  "$IMAGE:$TAG"

for i in $(seq 1 30); do
  [ "$(docker inspect -f '{{.State.Health.Status}}' "api-$NEXT")" = "healthy" ] && break
  sleep 2
done
if [ "$(docker inspect -f '{{.State.Health.Status}}' "api-$NEXT")" != "healthy" ]; then
  docker logs --tail 50 "api-$NEXT"; docker rm -f "api-$NEXT"
  echo "!! new color unhealthy; traffic never moved"; exit 1
fi

sed "s/api-[a-z]*:3000/api-$NEXT:3000/" Caddyfile.tmpl > Caddyfile
docker exec caddy caddy reload --config /etc/caddy/Caddyfile
echo "$NEXT" > .active_color
echo ">> traffic now on $NEXT (old color '$ACTIVE' kept for instant rollback)"
```

Rollback is the same script's mirror image: point Caddy back to the old color, which is still running.

### 7.5 Release management

Use **Semantic Versioning** (`MAJOR.MINOR.PATCH`) and let Conventional Commits drive it: `fix:` → patch, `feat:` → minor, `feat!:` or `BREAKING CHANGE:` → major.

📄 A tag-driven release (GitLab Releases page + immutable version tag on the image):

```yaml
release-image:
  stage: package
  image: docker:27
  services: ["docker:27-dind"]
  variables:
    DOCKER_TLS_CERTDIR: "/certs"
  before_script:
    - echo "$CI_REGISTRY_PASSWORD" | docker login -u "$CI_REGISTRY_USER" --password-stdin "$CI_REGISTRY"
  script:
    # retag the already-tested SHA image instead of rebuilding
    - docker pull "$CI_REGISTRY_IMAGE:$CI_COMMIT_SHORT_SHA"
    - docker tag  "$CI_REGISTRY_IMAGE:$CI_COMMIT_SHORT_SHA" "$CI_REGISTRY_IMAGE:$CI_COMMIT_TAG"
    - docker push "$CI_REGISTRY_IMAGE:$CI_COMMIT_TAG"
  rules:
    - if: $CI_COMMIT_TAG =~ /^v\d+\.\d+\.\d+$/

gitlab-release:
  stage: deploy
  image: registry.gitlab.com/gitlab-org/release-cli:latest   # check the docs for the current recommended image
  needs: [release-image]
  script: [echo "creating release for $CI_COMMIT_TAG"]
  release:
    tag_name: $CI_COMMIT_TAG
    description: "Release $CI_COMMIT_TAG. See the commit history for changes."
  rules:
    - if: $CI_COMMIT_TAG =~ /^v\d+\.\d+\.\d+$/
```

For fully automatic versions and changelogs from your commit messages, look at **semantic-release** with the GitLab plugin.

### 7.6 Supply-chain and pipeline security

| Risk | Control |
|---|---|
| Compromised or unpinned dependency/base image | pin versions and, for sensitive systems, image digests; use Renovate to update deliberately via MRs |
| Malicious change to CI config to steal secrets | protected branches, required approvals, **CODEOWNERS** on `.gitlab-ci.yml`, protected variables, protected environments |
| Leaked tokens | short-lived credentials (OIDC), masked + protected variables, secret scanning, rotation |
| Overpowered runners | separate runners for trusted/untrusted code; privileged (DinD) runners only where needed; ephemeral runners |
| Unknown contents of an image | generate an **SBOM** (`trivy image --format cyclonedx`), scan it, store it as an artifact |
| Tampered artifact | immutable tags, image signing (for example **cosign**), verify before deploy |
| `CI_JOB_TOKEN` abuse | restrict its access scope with the project's **job token allowlist** |
| Over-broad deploy rights | one role/key per environment, `sub` conditions on branch, manual gate for production |

📄 SBOM job example:

```yaml
sbom:
  stage: scan
  needs: [build-image]
  image:
    name: aquasec/trivy:0.57.0
    entrypoint: [""]
  variables:
    TRIVY_USERNAME: "$CI_REGISTRY_USER"
    TRIVY_PASSWORD: "$CI_REGISTRY_PASSWORD"
  script:
    - trivy image --format cyclonedx --output sbom.cdx.json "$CI_REGISTRY_IMAGE:$CI_COMMIT_SHORT_SHA"
  artifacts:
    paths: [sbom.cdx.json]
    expire_in: 30 days
  rules:
    - if: $CI_COMMIT_BRANCH == "main"
```

### 7.7 Measuring delivery performance (DORA metrics)

| Metric | Question | Rough "elite" direction |
|---|---|---|
| **Deployment frequency** | how often do we ship to production? | on demand, many per day |
| **Lead time for changes** | commit → running in production | under a day |
| **Change failure rate** | what share of deployments cause an incident or rollback? | low (roughly under 15%) |
| **Time to restore service** | how fast do we recover from a failure? | under an hour |

GitLab's built-in DORA dashboards depend on your plan; you can also calculate the numbers yourself from the Deployments API or your environment history. The goal is **trends**, not blame: faster, smaller, safer releases.

Related ideas: **SLI/SLO** (a measurable reliability target such as "99.5% of requests succeed within 500 ms") and an **error budget** (the allowed unreliability that, when spent, pauses risky releases).

### 7.8 GitOps and Kubernetes (where to go next)

In GitOps the **desired state of the cluster lives in Git**, and an agent (Argo CD, Flux) continuously reconciles the cluster to match. CI builds and tests the image, then opens an MR (or commits) that changes the image tag in a deployment repo; the agent applies it. Rollback is `git revert`. Learn after Level 7 with: containers → Kubernetes (Deployments, Services, Ingress, probes) → Helm/Kustomize → Argo CD.

### 🧠 Knowledge Check: Advanced

1. Why pin a `ref` when including a shared template?
2. Why does `tf-plan` use a different IAM role than `tf-apply`?
3. What is the difference between blue/green and canary?
4. How can a team deploy code that users cannot see yet?
5. Why are review apps risky, and how do you reduce the risk?
6. What does `strategy: depend` do on a trigger job?
7. Name the four DORA metrics and one way a pipeline improves each.
8. Why is "build once, promote the same artifact" important for supply-chain security?
9. What is Terraform state, and why must it be protected and locked?

<details>
<summary>Answers</summary>

1. A moving branch can change under you and break (or compromise) every consumer at once; a tag gives reproducible, reviewable upgrades.
2. Plans should be possible from any MR with minimal rights; applies change real infrastructure and must be restricted to `main` and a manual approval.
3. Blue/green switches **all** traffic between two full environments; canary sends a **small share** to the new version first and grows it based on metrics.
4. Feature flags: ship the code disabled, then enable it gradually at runtime.
5. They execute unreviewed code on real infrastructure. Isolate hosts, strip secrets and data, set resource limits, auto-stop, and restrict who can trigger them.
6. The parent job (and pipeline) takes the child pipeline's status, so a failed child fails the parent.
7. Frequency (automated deploys), lead time (fast pipelines, small MRs), change failure rate (quality gates, canaries), time to restore (one-click rollback, alerts).
8. The thing you tested and scanned is exactly what runs in production; rebuilding can silently change dependencies or base layers.
9. State maps your code to real resources and may contain secrets. Corruption or concurrent applies can destroy or duplicate infrastructure, so it needs encryption, access control, versioning, and locking.
</details>

### 🛠️ Labs: Level 7

**Lab 25: Shared templates and child pipelines**

- **Steps:**
  1. Create a second GitLab project `ci-templates` with `node.yml` (7.1). Tag it `v1.0.0`.
  2. Make `bookshelf-api` consume it via `include: project` with the pinned tag.
  3. Release `v1.1.0` with a deliberately broken job, and confirm the consumer is **unaffected** until it bumps the tag. Then bump it.
  4. Create a tiny monorepo experiment with two folders and parent/child pipelines using `rules: changes`.
- ✅ **Done when:** a change in only one folder triggers only that child pipeline, and your consumer repo controls when it upgrades templates.

**Lab 26: Review apps**

- **Steps:** Add the review jobs (7.2). Open an MR, click the environment link in the MR, then merge and verify that the environment stops (or use the stop button).
- ✅ **Done when:** each MR gets its own URL, and closing or merging cleans it up.
- ⚠️ Use an isolated host, never your production server.

**Lab 27: Release automation**

- **Steps:** Add the tag-driven release jobs (7.5). Create the tag `v1.0.0` from `main`, then `v1.1.0` after a `feat:` commit. Verify the GitLab *Releases* page and the version-tagged image in the registry.
- ✅ **Done when:** pushing a `vX.Y.Z` tag publishes a release and re-tags **the already-tested image** without rebuilding it.
- 🚀 **Stretch:** Replace manual tagging with semantic-release driven by Conventional Commits.

**Lab 28: Terraform your AWS resources**

- **Steps:**
  1. Create the state bucket by hand (versioning + encryption + public access blocked).
  2. Write `infra/main.tf` (7.3). `terraform import` your existing ECR repository, or destroy it first and let Terraform create it.
  3. Add `tf-validate`, `tf-plan`, `tf-apply`. Open an MR that changes the log-retention days and read the plan in the job log.
  4. Merge and apply manually.
  5. Try running `terraform apply` locally at the same time as a pipeline apply, and observe the **state lock**.
- ✅ **Done when:** every infrastructure change goes through an MR with a visible plan, and no one needs the console to change it.
- 🚀 **Stretch:** Codify the ECS service and ALB in Terraform, then run `terraform destroy` for a fast, safe teardown.

**Lab 29: Blue/green deployment**

- **Steps:** Implement `bluegreen.sh` (7.4) on your staging server. Deploy v1, then v2: confirm users see no errors with `while true; do curl -s -o /dev/null -w "%{http_code}\n" http://SERVER/health; sleep 0.2; done`. Then roll back by switching Caddy back.
- ✅ **Done when:** you see an uninterrupted stream of `200` during the switch and rollback takes seconds.

**Lab 30: Harden the supply chain**

- **Steps:**
  1. Pin base images to exact versions (and digests for one image). Add Renovate or Dependabot-style update MRs if you can.
  2. Add the SBOM job. Open the SBOM and find one package you did not know you shipped.
  3. Add **CODEOWNERS** so `.gitlab-ci.yml`, `Dockerfile`, and `infra/` require a specific reviewer.
  4. Restrict the **job token allowlist** and audit which variables are Protected.
  5. Write a one-page threat model: "if an attacker could change a CI file in a feature branch, what could they reach?" List the controls that stop each path.
- ✅ **Done when:** the threat-model document maps every attack path you identified to a control you actually configured.

**Lab 31: DORA metrics and a game day**

- **Steps:**
  1. Collect your numbers: deployments in the last 2 weeks, median commit→production time, failed deployments, restore time of your last rollback.
  2. Hold a solo **game day**: a teammate (or you) secretly makes one change that breaks production in a realistic way (bad config, expired secret, full disk, unhealthy dependency). Detect it from your alarms/logs, restore service, then write a blameless postmortem (template below).
- ✅ **Done when:** you have a metrics table and a completed postmortem with at least two concrete follow-up actions.

---

## Real-world workflows

### Workflow 1: Feature to production (the reference flow)

```mermaid
flowchart TD
    A["Ticket / issue"] --> B["feat/ branch from test"]
    B --> C["Commit with Conventional Commits<br/>(pre-commit hooks)"]
    C --> D["Push (pre-push checks)"]
    D --> E["Merge Request into test"]
    E --> F{"Pipeline green?<br/>lint, tests, SAST, secrets"}
    F -- "no" --> C
    F -- "yes" --> G{"Review approved?<br/>CODEOWNERS satisfied"}
    G -- "no" --> C
    G -- "yes" --> H["Squash merge into test"]
    H --> I["MR test into stage"]
    I --> J["Build image once, scan, push to registry"]
    J --> K["Auto-deploy to STAGING + smoke tests"]
    K --> L{"QA / UAT OK?"}
    L -- "no" --> C
    L -- "yes" --> M["MR stage into main (2 approvals)"]
    M --> N["Same image promoted"]
    N --> O["Manual approval"]
    O --> P["Deploy to PRODUCTION (rolling or blue/green)"]
    P --> Q{"Health + alarms OK?"}
    Q -- "yes" --> R["Tag release vX.Y.Z"]
    Q -- "no" --> S["Auto / manual rollback"]
    S --> C
```

### Workflow 2: Hotfix

1. Alert fires → on-call acknowledges and starts an incident channel.
2. **Mitigate first** (roll back, disable a flag, scale up), then fix properly.
3. `git switch -c hotfix/payment-crash main` → minimal fix **plus a regression test**.
4. MR into `main`: full pipeline, expedited review by one senior reviewer.
5. Manual production deploy → verify metrics.
6. **Back-merge** `main` into `stage` and `test`, otherwise the fix vanishes in the next promotion.
7. Blameless postmortem within a few days.

### Workflow 3: Rollback decision

```mermaid
flowchart TD
    A["Alert after a deploy"] --> B{"Deployed within the last hour?"}
    B -- "yes" --> C["Roll back to previous SHA / flip feature flag"]
    B -- "no" --> D["Investigate logs, metrics, dependencies"]
    C --> E{"Recovered?"}
    E -- "yes" --> F["Investigate calmly, fix forward via normal pipeline"]
    E -- "no" --> D
    D --> G{"DB schema changed?"}
    G -- "yes" --> H["Check migration is backward-compatible before rolling code back"]
    G -- "no" --> C
```

**Rule of thumb:** when production is on fire, **restore service first, find the root cause second.**

### Workflow 4: Dependency and base-image updates

Renovate (or Dependabot) opens small MRs. Your pipeline's tests and scans decide whether it is safe. Auto-merge only patch-level updates that are green; review minors and majors. A nightly scheduled pipeline rebuilds images to pick up OS patches.

### Workflow 5: Code review standards

Reviewers check, in order: **correctness**, **security** (input validation, authorization, secrets), **tests**, **readability**, **operations** (logging, error handling, migrations, config, performance). Keep MRs small (a few hundred lines). Comments are about the code, not the person. The author responds to every thread. Use *suggestions* for tiny fixes.

### Workflow 6: Blameless postmortem template

📄 `postmortems/YYYY-MM-DD-title.md`

```markdown
# Postmortem: <short title>

- **Date / duration:** 2026-11-24, 14:05 to 14:42 (37 min)
- **Impact:** who/what was affected, how badly (for example 30% of requests failed)
- **Detection:** how we found out (alarm, user report) and how long it took

## Timeline (UTC)
- 14:03 Deploy of 3f2a9c1 started
- 14:05 5xx alarm fired
- 14:12 On-call acknowledged
- 14:20 Decided to roll back
- 14:42 Service restored

## Root cause
<the underlying cause, not the person>

## What went well / what went badly / where we got lucky

## Action items
| Action | Owner | Due |
|---|---|---|
| Add a smoke test covering X | name | date |
| Alert on Y earlier | name | date |
```

### Workflow 7: Environment and access model

| Environment | Branch | Deploy | Who can trigger | Data | Secrets |
|---|---|---|---|---|---|
| Review app | MR | automatic | any developer | fake | none or fake |
| Test | `test` | automatic | any developer | fake | test-only |
| Staging | `stage` | automatic | maintainers merge | anonymised copy or seed | staging-only |
| Production | `main` | **manual approval** | maintainers | real | production-only, protected |

---

## Capstone project

**Build and run a complete delivery platform for `bookshelf-api`**, as if it were a client project. Put the evidence in a `docs/` folder and a README that a new teammate could follow.

### Requirements

| # | Area | Requirement |
|---|---|---|
| 1 | Git | `test → stage → main` branches protected; Conventional Commits enforced locally and in CI; MR template; CODEOWNERS |
| 2 | Quality | lint, format, unit and integration tests (Postgres service), coverage threshold, matrix on two Node versions |
| 3 | Security | secret detection, SAST, dependency audit, image scan (blocking on critical), SBOM artifact, written `SECURITY_POLICY.md` |
| 4 | Docker | multi-stage, non-root, health-checked image; built **once** per commit; tagged by SHA; registry cleanup policy |
| 5 | Staging | auto-deploy on `stage`, smoke test job, automatic rollback on failed health check |
| 6 | Production | manual approval, protected environment/variables, `resource_group`, deploy to **AWS ECS Fargate** using **OIDC** (no stored keys) |
| 7 | IaC | ECR, log group, and (stretch) ECS/ALB defined in Terraform with plan on MR and manual apply on `main` |
| 8 | Observability | structured JSON logs, CloudWatch log retention, health endpoint, at least 3 alarms with email notification |
| 9 | Data | automated database backup copied to S3, and a **documented, timed restore** |
| 10 | Operations | `RUNBOOK.md` (deploy, rollback, logs, restore, contacts), one hotfix drill, one rollback drill, one postmortem |
| 11 | Cost | budget alarm, `CLEANUP.md`, and the final environment torn down |
| 12 | Measurement | DORA metrics table for the capstone period |

### Rubric (self-assess honestly)

| Score | Meaning |
|---|---|
| 0 | not attempted |
| 1 | works on my machine / manual steps |
| 2 | automated, but fragile or undocumented |
| 3 | automated, documented, **and** failure-tested (you broke it on purpose and it behaved) |

You are "industry ready" for these topics when most rows score 3.

### Capstone deliverables checklist

- [ ] Repository with protected branches and green pipeline on `main`
- [ ] Pipeline diagram (Mermaid) in the README
- [ ] Screenshot or log of: a blocked insecure change, a successful production deploy, an automatic rollback
- [ ] `RUNBOOK.md`, `SECURITY_POLICY.md`, `CLEANUP.md`, one postmortem
- [ ] A 5-minute walkthrough recording or write-up explaining your design decisions and trade-offs

---

## Interview-style questions

Answer out loud or in writing, then check the hints.

<details>
<summary><strong>Git and workflow</strong></summary>

1. Compare trunk-based development with environment branches. When is each a better fit?
   *Hint: trunk-based needs strong tests and feature flags and ships fastest; environment branches give explicit promotion gates but risk drift and merge pain.*
2. How do you prevent unreviewed code from reaching production?
   *Hint: protected branches, required approvals, CODEOWNERS, pipelines must succeed, protected environments, no direct pushes.*
3. A teammate force-pushed over `stage`. How do you recover and prevent it?
   *Hint: reflog on someone's clone or the remote's reflog/API, restore the branch to the previous SHA; then disable force push on protected branches.*
</details>

<details>
<summary><strong>CI/CD design</strong></summary>

4. A pipeline takes 35 minutes. How do you reduce it?
   *Hint: measure first; `needs`/DAG, parallelism, caching, smaller images, run slow tests only where useful, change-based rules, interruptible, faster runners.*
5. How do you stop secrets leaking through CI?
   *Hint: no secrets in Git, masked + protected variables, OIDC instead of keys, secret scanning, restricted CI-file edits, least-privilege tokens.*
6. Explain build once, deploy many.
   *Hint: one immutable image promoted unchanged; only configuration differs per environment.*
7. How would you implement zero-downtime deployment?
   *Hint: rolling or blue/green with health checks and graceful shutdown; backward-compatible DB migrations.*
8. What would you check first for a pipeline that is green but a broken app reached production?
   *Hint: missing smoke tests, tests that don't cover the failing path, config differences between environments, a manual or ignored `allow_failure` job.*
</details>

<details>
<summary><strong>Docker</strong></summary>

9. Why might a container be fine locally but crash on a server?
   *Hint: architecture mismatch (arm64 vs amd64), missing env vars, different permissions/user, volumes, network, memory limits.*
10. How do you keep an image small and secure?
    *Hint: multi-stage, minimal base, no dev dependencies, non-root, `.dockerignore`, pinned and regularly updated base, scanning.*
11. What happens to data written inside a container when it is removed?
    *Hint: it is lost unless stored in a volume or external service.*
</details>

<details>
<summary><strong>AWS</strong></summary>

12. Walk me through how a GitLab job gets permission to deploy to ECS without stored keys.
    *Hint: OIDC token → STS AssumeRoleWithWebIdentity → trust-policy `aud`/`sub` conditions → temporary credentials → least-privilege role.*
13. A new ECS task keeps stopping with `CannotPullContainerError`. What do you check?
    *Hint: execution role permissions for ECR, network path to ECR (public IP, NAT, or VPC endpoints), image tag exists, region/account correct.*
14. The ALB returns 503 and targets are unhealthy. What do you check?
    *Hint: health-check path/port, security-group rules ALB → task, container actually listening on the expected port, startup time vs health-check grace period, app logs.*
15. How do you control AWS costs in a learning or dev environment?
    *Hint: budgets and alarms, tagging, teardown scripts/Terraform destroy, avoid NAT/ALB left running, log retention, lifecycle rules, scheduled shutdowns.*
</details>

<details>
<summary><strong>Operations and reliability</strong></summary>

16. What do you monitor for a web API?
    *Hint: golden signals (latency, traffic, errors, saturation), plus business metrics, dependency health, certificate expiry, disk, backups success.*
17. Explain RPO and RTO.
    *Hint: RPO = how much data you can afford to lose; RTO = how long you can afford to be down. They drive backup frequency and recovery design.*
18. Production is down after a deploy. First three actions?
    *Hint: declare incident and communicate; restore service (rollback/flag/scale); then investigate root cause and write a postmortem.*
19. How do you roll back a release that included a database migration?
    *Hint: you generally can't undo data changes, so use expand/migrate/contract with backward-compatible schemas; keep backups; test rollbacks.*
20. What makes a test flaky, and how do you manage it?
    *Hint: shared state, timing, randomness, network; quarantine and fix, track flaky rate, never mask with blind retries.*
</details>

---

## Pipeline debugging playbook

**Method:** (1) read the *first* error in the job log, not the last; (2) reproduce locally with the same image and commands; (3) change one thing at a time; (4) write down the fix in `LEARNING_LOG.md`.

| Symptom | Likely cause | Fix |
|---|---|---|
| Job stuck in **pending** | no runner matches the job's `tags`, or runners are busy or out of quota | remove or correct `tags`; check *Settings → CI/CD → Runners* and your usage quota |
| `npm ci` fails: *can only install with an existing package-lock.json* | lock file not committed or out of sync | run `npm install` locally and commit `package-lock.json` |
| Job passes locally, fails in CI | different Node version, missing env var, OS differences, case-sensitive paths | run the CI image locally: `docker run --rm -it -v "$PWD":/app -w /app node:22-alpine sh` |
| Pipeline runs twice per push | both branch and MR pipelines | add `workflow:rules` (section 2.4) |
| `Cannot connect to the Docker daemon at tcp://docker:2375` | DinD service missing, TLS variables mismatch, or runner is not privileged | use `services: [docker:27-dind]` with `DOCKER_TLS_CERTDIR: "/certs"`; confirm the runner supports privileged mode |
| `denied: requested access to the resource is denied` (registry push) | not logged in, wrong image path, or insufficient permission | check `docker login` step and that the image name begins with `$CI_REGISTRY_IMAGE` |
| `exec format error` | CPU architecture mismatch (arm64 image on amd64 host or reverse) | build for the target platform: `docker buildx build --platform linux/amd64 ...` |
| `exit code 137` | out of memory (OOM kill) | reduce memory use, split the job, use a bigger runner, limit parallel workers |
| `Permission denied (publickey)` on deploy | wrong key, key not in `authorized_keys`, wrong user, key file permissions | verify `SSH_USER`, public key on the server, `chmod 600` on the key (File-type variable) |
| `Host key verification failed` | `known_hosts` missing or server rebuilt | refresh `SSH_KNOWN_HOSTS` with `ssh-keyscan` and verify the fingerprint out of band |
| Variable is empty in the job | variable is **Protected** but the branch is not, wrong environment scope, or a typo | align protection/scope with the branch or environment |
| `Not authorized to perform sts:AssumeRoleWithWebIdentity` | `aud` or `sub` mismatch, wrong provider URL, branch not allowed by trust policy | print the decoded token claims (header/payload only, never share the token) and compare with the trust policy conditions |
| `AccessDenied` on an ECR/ECS call | role policy too narrow, wrong resource ARN, missing `iam:PassRole` | read the error's action and resource; add exactly that |
| ECS service never stabilizes | container crashes, failing health check, can't pull image, not enough capacity | service **Events** tab, stopped-task reason, CloudWatch logs, target-group health |
| ALB `502/503` | no healthy targets or app not listening on the port | match container port, target-group port and health path; check security groups |
| Cache never hits | cache key changes every run, or jobs run on different runners without shared cache | use a stable key (`files: [package-lock.json]`) and check the cache logs |
| Husky tries to run in CI | `prepare` script runs on `npm ci` | set `HUSKY=0` in CI variables |
| `rejected ... (non-fast-forward)` | remote has commits you do not | `git pull --rebase` and push again; never force-push shared branches |
| Terraform: *Error acquiring the state lock* | another run is in progress or crashed | wait, or `terraform force-unlock <ID>` only after confirming nothing is running |

---

## Cheat sheets

### GitLab CI keywords

| Keyword | Purpose |
|---|---|
| `stages` | ordered groups of jobs |
| `image`, `services` | container for the job; sidecars (databases) |
| `script`, `before_script`, `after_script` | commands |
| `variables` | environment variables |
| `rules` | when to run (`if`, `changes`, `exists`, `when`, `allow_failure`) |
| `needs` | DAG dependencies, start early |
| `artifacts` | files kept and passed on; `reports` for test/coverage |
| `cache` | reuse downloaded dependencies |
| `environment` | track deployments, review apps, stop actions |
| `resource_group` | serialize deployments |
| `extends`, `include`, `!reference` | reuse and compose config |
| `parallel`, `matrix` | duplicate jobs across values |
| `trigger` | child or downstream pipelines |
| `id_tokens` | OIDC tokens for cloud authentication |
| `interruptible`, `timeout`, `retry` | efficiency and reliability |
| `workflow:rules` | control whether a pipeline exists at all |

### Git

```bash
git switch -c feat/x test           # branch from test
git add -p                          # stage interactively
git commit -m "feat(x): ..."
git fetch origin && git rebase origin/test
git push -u origin feat/x
git log --oneline --graph --all
git reflog                          # find "lost" commits
git revert <sha>                    # safe undo of a pushed commit
git stash push -m "wip" ; git stash pop
git cherry-pick <sha>
git rebase -i HEAD~3                # squash/reword
git tag -a v1.0.0 -m "msg" && git push origin v1.0.0
```

### Docker

```bash
docker build -t app:tag .            docker run --rm -p 3000:3000 app:tag
docker ps -a                         docker logs -f <c>
docker exec -it <c> sh               docker inspect <c>
docker compose up -d --build         docker compose logs -f <svc>
docker compose down -v               docker system df ; docker image prune -f
docker login <registry>              docker push/pull <image>:<tag>
```

### AWS CLI

```bash
aws sts get-caller-identity
aws ecr describe-images --repository-name bookshelf-api
aws ecs describe-services --cluster bookshelf --services bookshelf-api
aws ecs list-tasks --cluster bookshelf --service-name bookshelf-api
aws logs tail /ecs/bookshelf-api --follow
aws s3 ls s3://bucket/ ; aws s3 cp file s3://bucket/
aws ce get-cost-and-usage ...        # cost explorer (see docs)
```

### Linux

```bash
ls -lah ; cd - ; pwd              grep -rn "text" . ; find . -name "*.js"
tail -f file ; less file          chmod 600 key ; chown user:group path
ps aux | grep x ; kill PID        ss -tulpn ; curl -i URL
df -h ; free -m ; du -sh *        systemctl status|restart svc ; journalctl -u svc -f
ssh user@host ; scp f user@host:/p
```

---

## Progress tracker

Copy this table into your `LEARNING_LOG.md` and tick items as you finish. A lab counts only when its **✅ Done when** line is true.

| Level | Labs | Done |
|---|---|---|
| 0: Setup | accounts, AWS safety, tools, repo | ☐ |
| 1: Linux and Git | Lab 1, 2, 3, 4 | ☐ ☐ ☐ ☐ |
| 2: CI basics | Lab 5, 6, 7, 8, 9 | ☐ ☐ ☐ ☐ ☐ |
| 3: Testing and security | Lab 10, 11, 12, 13 | ☐ ☐ ☐ ☐ |
| 4: Docker | Lab 14, 15, 16, 17 | ☐ ☐ ☐ ☐ |
| 5: Server deploy | Lab 18, 19, 20 | ☐ ☐ ☐ |
| 6: AWS | Lab 21, 22, 23, 24 | ☐ ☐ ☐ ☐ |
| 7: Advanced | Lab 25, 26, 27, 28, 29, 30, 31 | ☐ ☐ ☐ ☐ ☐ ☐ ☐ |
| Capstone | all 12 requirements ≥ 2, most at 3 | ☐ |

### Where to go next

- **Kubernetes and GitOps:** k3s or a managed cluster, Helm, Argo CD.
- **Observability:** Prometheus + Grafana, OpenTelemetry tracing, SLO dashboards.
- **Platform engineering:** internal CI component catalogs, self-service environments.
- **Certifications worth considering:** AWS Cloud Practitioner → Solutions Architect Associate → DevOps Engineer Professional; Certified Kubernetes Administrator (CKA).
- **Primary documentation to bookmark:** GitLab CI/CD docs, Docker docs, AWS docs (IAM, ECS, ECR), Terraform docs. GitLab and AWS change often, so when this guide and the official docs disagree, trust the docs and update your notes.

*Happy shipping. Break things on purpose, read the logs, and write down what you learn.*

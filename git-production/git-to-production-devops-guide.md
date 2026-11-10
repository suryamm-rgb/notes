# Git → CI/CD → Docker → Production: The Complete Workflow Guide

> For developers who already know frontend/backend and want to learn the professional delivery pipeline end to end.
> Stack used in examples: **Node.js + TypeScript**, **GitLab** (Merge Requests, GitLab CI), **Docker**, **Linux VPS**.
> GitHub equivalents are noted where they differ (Pull Requests, GitHub Actions).

---

## Table of Contents

1. [The Big Picture](#1-the-big-picture)
2. [Git Branching Strategy: test → stage → main](#2-git-branching-strategy-test--stage--main)
3. [Conventional Commits](#3-conventional-commits)
4. [Local Quality Gates (Husky hooks)](#4-local-quality-gates-husky-hooks)
5. [CI/CD Pipeline](#5-cicd-pipeline)
6. [Branch Protection and Code Review](#6-branch-protection-and-code-review)
7. [Docker for Production](#7-docker-for-production)
8. [Deployment to Staging and Production](#8-deployment-to-staging-and-production)
9. [Production Readiness](#9-production-readiness)
10. [Learning Roadmap](#10-learning-roadmap)
11. [10 Practice Tasks](#11-10-practice-tasks)
12. [Cheat Sheet](#12-cheat-sheet)

---

## 1. The Big Picture

```
 Developer machine                  Git server (GitLab)                    Servers
┌──────────────────┐   push    ┌──────────────────────────┐        ┌──────────────────┐
│ feature branch   │ ───────▶  │ MR → test   (CI runs)    │        │                  │
│ pre-commit hook  │           │ MR → stage  (CI + deploy)│ ─────▶ │ Staging server   │
│ pre-push hook    │           │ MR → main   (CI + deploy)│ ─────▶ │ Production server│
└──────────────────┘           └──────────────────────────┘        └──────────────────┘
```

**Core idea:** every change passes through progressively stricter gates, and no gate can be skipped.

| Gate | Where | What it catches |
|---|---|---|
| Pre-commit | Your machine | Formatting, lint errors, type errors in staged files, bad commit messages |
| Pre-push | Your machine | Broken tests, broken build |
| CI pipeline | Git server | Everything above again (hooks can be bypassed; CI cannot) |
| Merge Request review | Git server | Logic bugs, design problems, security issues |
| Staging deploy | Staging server | Environment-specific issues, integration problems |
| Production deploy | Production server | Final, approved release |

> **Rule:** Local hooks are for *speed* (fast feedback). CI is for *enforcement* (cannot be skipped).

---

## 2. Git Branching Strategy: test → stage → main

### 2.1 Branch roles

| Branch | Purpose | Deploys to | Who can merge |
|---|---|---|---|
| `feat/*`, `fix/*`, etc. | Individual work | nothing | author |
| `test` | Integration of all developers' work | Test/dev environment (optional) | Developers (via MR) |
| `stage` | Release candidate, QA / UAT | Staging server | Maintainers / Tech leads |
| `main` | Production truth | Production server | Maintainers only, with approvals |

### 2.2 Flow

```
feat/login ──┐
fix/cart-bug ─┼──▶ test ──▶ stage ──▶ main
refactor/api ─┘      (MR)     (MR)     (MR)
```

- Branch **from `test`** for normal work.
- Promote via Merge Requests: `test → stage → main`.
- Code only ever moves **forward**. Never commit directly to `stage` or `main`.

### 2.3 Hotfix flow (production is broken)

```
main ──▶ hotfix/payment-crash ──▶ MR → main (fast, still needs review + CI)
                                       │
                          then merge main back into stage and test
```

Always back-merge so the fix isn't lost in the next promotion.

### 2.4 Branch naming

```
feat/user-authentication
fix/login-redirect-loop
refactor/order-service
chore/update-dependencies
docs/api-readme
hotfix/payment-crash
```

### 2.5 Daily commands

```bash
# Start work
git switch test
git pull origin test
git switch -c feat/user-profile

# Work, commit (hooks run automatically)
git add .
git commit -m "feat(profile): add avatar upload"

# Keep your branch current
git fetch origin
git rebase origin/test          # or: git merge origin/test

# Push and open a Merge Request into `test`
git push -u origin feat/user-profile
```

### 2.6 Merge vs Rebase vs Squash

| Strategy | Result | Use for |
|---|---|---|
| **Squash merge** | One clean commit per MR | Feature → `test` (recommended) |
| **Merge commit** | Preserves branch history | `test → stage → main` promotions |
| **Rebase** | Linear history | Updating your own feature branch |

> **Golden rule:** never rebase a branch others are working from.

---

## 3. Conventional Commits

### 3.1 Format

```
<type>(<optional scope>): <short description>

[optional body]

[optional footer]
```

### 3.2 Types

| Type | Use for | Version bump |
|---|---|---|
| `feat` | New feature | minor |
| `fix` | Bug fix | patch |
| `refactor` | Code change, no behavior change | none |
| `chore` | Tooling, deps, config | none |
| `docs` | Documentation | none |
| `test` | Adding/fixing tests | none |
| `perf` | Performance improvement | patch |
| `ci` | CI/CD changes | none |
| `build` | Build system, Docker | none |
| `style` | Formatting only | none |
| `revert` | Reverting a commit | varies |

### 3.3 Examples

```
feat(auth): add Google OAuth login
fix(cart): prevent negative quantity on update
refactor(orders): extract pricing into separate service
chore(deps): upgrade typescript to 5.6
feat(api)!: remove v1 endpoints

BREAKING CHANGE: /api/v1/* routes no longer exist, use /api/v2/*
```

`!` or a `BREAKING CHANGE:` footer signals a major version bump.

### 3.4 Enforce with commitlint

```bash
npm i -D @commitlint/cli @commitlint/config-conventional
```

`commitlint.config.js`:

```js
export default {
  extends: ['@commitlint/config-conventional'],
  rules: {
    'subject-case': [2, 'never', ['upper-case', 'pascal-case', 'start-case']],
    'header-max-length': [2, 'always', 72],
  },
};
```

### 3.5 Why bother?

- Readable history
- Automatic changelogs and version numbers (`semantic-release`, `standard-version`, `release-please`)
- Easier code review and rollback

---

## 4. Local Quality Gates (Husky hooks)

### 4.1 Install tooling

```bash
npm i -D husky lint-staged prettier eslint typescript \
         typescript-eslint @eslint/js eslint-config-prettier vitest

npx husky init
```

### 4.2 `package.json` scripts

```json
{
  "scripts": {
    "dev": "tsx watch src/index.ts",
    "build": "tsc -p tsconfig.build.json",
    "start": "node dist/index.js",
    "lint": "eslint . --max-warnings=0",
    "lint:fix": "eslint . --fix",
    "format": "prettier --write .",
    "format:check": "prettier --check .",
    "typecheck": "tsc --noEmit",
    "test": "vitest run",
    "test:coverage": "vitest run --coverage",
    "prepare": "husky"
  },
  "lint-staged": {
    "*.{ts,tsx,js,jsx}": ["eslint --fix --max-warnings=0", "prettier --write"],
    "*.{json,md,yml,yaml,css}": ["prettier --write"]
  }
}
```

### 4.3 ESLint (flat config) – `eslint.config.js`

```js
import js from '@eslint/js';
import tseslint from 'typescript-eslint';
import prettier from 'eslint-config-prettier';

export default tseslint.config(
  { ignores: ['dist', 'coverage', 'node_modules'] },
  js.configs.recommended,
  ...tseslint.configs.recommended,
  prettier,
  {
    rules: {
      '@typescript-eslint/no-unused-vars': ['error', { argsIgnorePattern: '^_' }],
      '@typescript-eslint/no-explicit-any': 'warn',
      'no-console': ['warn', { allow: ['warn', 'error'] }],
    },
  },
);
```

### 4.4 Prettier – `.prettierrc`

```json
{
  "semi": true,
  "singleQuote": true,
  "trailingComma": "all",
  "printWidth": 100
}
```

### 4.5 Hooks

**`.husky/pre-commit`** (fast: only staged files)

```sh
npx lint-staged
npm run typecheck
```

> `tsc` can't check only staged files, so it checks the whole project. If it's too slow on a large repo, move it to pre-push.

**`.husky/commit-msg`**

```sh
npx --no -- commitlint --edit "$1"
```

**`.husky/pre-push`** (thorough: whole project)

```sh
npm run lint
npm run typecheck
npm run test
npm run build
```

### 4.6 Bypass (emergencies only)

```bash
git commit --no-verify
git push --no-verify
```

This is why CI must repeat every check.

---

## 5. CI/CD Pipeline

### 5.1 Concepts

- **CI (Continuous Integration):** automatically verify every push (lint, type-check, test, build).
- **CD (Continuous Delivery/Deployment):** automatically package and deploy verified code.
  - *Delivery*: deployment to production needs a manual click.
  - *Deployment*: fully automatic.

### 5.2 Pipeline design

```
Push / MR
   │
   ▼
[quality]   lint ─ format:check ─ typecheck        (parallel)
   │
   ▼
[test]      unit/integration tests + coverage
   │
   ▼
[build]     npm build  →  docker build  →  push image to registry
   │
   ▼
[deploy]    test branch:  (optional) deploy to test env
            stage branch: auto deploy to STAGING
            main branch:  manual approval → deploy to PRODUCTION
```

### 5.3 `.gitlab-ci.yml`

```yaml
stages:
  - quality
  - test
  - build
  - deploy

workflow:
  rules:
    - if: $CI_PIPELINE_SOURCE == "merge_request_event"
    - if: $CI_COMMIT_BRANCH =~ /^(test|stage|main)$/

default:
  image: node:20-alpine
  cache:
    key:
      files: [package-lock.json]
    paths: [.npm/]

variables:
  NPM_CONFIG_CACHE: "$CI_PROJECT_DIR/.npm"
  IMAGE: "$CI_REGISTRY_IMAGE"

.install: &install
  before_script:
    - npm ci --prefer-offline

# ---------- QUALITY ----------
lint:
  stage: quality
  <<: *install
  script: npm run lint

format:
  stage: quality
  <<: *install
  script: npm run format:check

typecheck:
  stage: quality
  <<: *install
  script: npm run typecheck

audit:
  stage: quality
  script: npm audit --audit-level=high
  allow_failure: true

# ---------- TEST ----------
unit-tests:
  stage: test
  <<: *install
  script: npm run test:coverage
  coverage: '/All files[^|]*\|[^|]*\s+([\d\.]+)/'
  artifacts:
    when: always
    paths: [coverage/]
    expire_in: 7 days

# ---------- BUILD ----------
build-app:
  stage: build
  <<: *install
  script: npm run build
  artifacts:
    paths: [dist/]
    expire_in: 1 day

build-image:
  stage: build
  image: docker:27
  services: [docker:27-dind]
  variables:
    DOCKER_TLS_CERTDIR: "/certs"
  before_script:
    - echo "$CI_REGISTRY_PASSWORD" | docker login -u "$CI_REGISTRY_USER" --password-stdin $CI_REGISTRY
  script:
    - docker build -t $IMAGE:$CI_COMMIT_SHORT_SHA -t $IMAGE:$CI_COMMIT_BRANCH .
    - docker push $IMAGE:$CI_COMMIT_SHORT_SHA
    - docker push $IMAGE:$CI_COMMIT_BRANCH
  rules:
    - if: $CI_COMMIT_BRANCH =~ /^(stage|main)$/

# ---------- DEPLOY ----------
.deploy:
  stage: deploy
  image: alpine:3.20
  before_script:
    - apk add --no-cache openssh-client
    - eval $(ssh-agent -s)
    - echo "$SSH_PRIVATE_KEY" | tr -d '\r' | ssh-add -
    - mkdir -p ~/.ssh && chmod 700 ~/.ssh
    - echo "$SSH_KNOWN_HOSTS" >> ~/.ssh/known_hosts
  script:
    - ssh $SSH_USER@$SSH_HOST "IMAGE=$IMAGE TAG=$CI_COMMIT_SHORT_SHA /opt/app/deploy.sh"

deploy-staging:
  extends: .deploy
  environment:
    name: staging
    url: https://staging.example.com
  rules:
    - if: $CI_COMMIT_BRANCH == "stage"

deploy-production:
  extends: .deploy
  environment:
    name: production
    url: https://example.com
  rules:
    - if: $CI_COMMIT_BRANCH == "main"
      when: manual          # human approval before prod
```

Set `SSH_PRIVATE_KEY`, `SSH_KNOWN_HOSTS`, `SSH_USER`, `SSH_HOST` in **Settings → CI/CD → Variables**, marking them **Masked** and **Protected**, and scoped per environment (staging vs production values).

### 5.4 GitHub Actions equivalent (quality job)

```yaml
# .github/workflows/ci.yml
name: CI
on:
  pull_request:
    branches: [test, stage, main]
  push:
    branches: [test, stage, main]

jobs:
  quality:
    runs-on: ubuntu-latest
    steps:
      - uses: actions/checkout@v4
      - uses: actions/setup-node@v4
        with: { node-version: 20, cache: npm }
      - run: npm ci
      - run: npm run lint
      - run: npm run format:check
      - run: npm run typecheck
      - run: npm test
      - run: npm run build
```

---

## 6. Branch Protection and Code Review

### 6.1 GitLab settings

**Settings → Repository → Protected branches**

| Branch | Allowed to merge | Allowed to push | Force push |
|---|---|---|---|
| `main` | Maintainers | **No one** | Off |
| `stage` | Maintainers (or Developers+Maintainers) | **No one** | Off |
| `test` | Developers + Maintainers | **No one** | Off |

**Settings → Merge requests**

- ✅ Pipelines must succeed
- ✅ All threads must be resolved
- ✅ Squash commits when merging (for feature → `test`)
- ✅ Fast-forward or merge commit (for promotions)

**Settings → Merge request approvals**

| Target branch | Required approvals | Who |
|---|---|---|
| `test` | 1 | Any developer |
| `stage` | 1–2 | Tech leads |
| `main` | 2 | Maintainers / code owners |

- ✅ Prevent approval by the MR author
- ✅ Remove approvals when new commits are pushed

**Protected environments** (Settings → CI/CD → Environments): restrict who can click "deploy" on `production`.

### 6.2 CODEOWNERS

`.gitlab/CODEOWNERS` (GitHub: `.github/CODEOWNERS`):

```
# Default owners
*                       @team/developers

# Sensitive areas need specific reviewers
/Dockerfile             @team/devops
/.gitlab-ci.yml         @team/devops
/src/auth/              @team/security
/db/migrations/         @team/backend-leads
```

### 6.3 GitHub equivalent

**Settings → Branches → Branch protection rules**: require PR, required approvals, required status checks (`quality`), require conversation resolution, restrict who can push, include administrators.

### 6.4 Merge Request template

`.gitlab/merge_request_templates/default.md`:

```markdown
## What does this change?
<!-- Short summary -->

## Why?
<!-- Link to issue/ticket -->

## How to test
1.
2.

## Checklist
- [ ] Tests added/updated
- [ ] No secrets or debug code
- [ ] Docs updated (if needed)
- [ ] DB migration is reversible (if applicable)
- [ ] Screenshots attached (UI changes)
```

### 6.5 What a good reviewer checks

1. **Correctness:** does it do what the ticket says? Edge cases?
2. **Security:** input validation, authz checks, no secrets, no injection risks.
3. **Tests:** meaningful, not just coverage padding.
4. **Readability:** naming, size of functions, comments explaining *why*.
5. **Operations:** logging, error handling, migrations, config changes, performance.

Keep MRs small (under ~400 changed lines). Big MRs get rubber-stamped.

---

## 7. Docker for Production

### 7.1 Multi-stage `Dockerfile`

```dockerfile
# syntax=docker/dockerfile:1

# ---------- Stage 1: dependencies ----------
FROM node:20-alpine AS deps
WORKDIR /app
COPY package.json package-lock.json ./
RUN npm ci

# ---------- Stage 2: build ----------
FROM node:20-alpine AS build
WORKDIR /app
COPY --from=deps /app/node_modules ./node_modules
COPY . .
RUN npm run build

# ---------- Stage 3: production deps only ----------
FROM node:20-alpine AS prod-deps
WORKDIR /app
COPY package.json package-lock.json ./
RUN npm ci --omit=dev && npm cache clean --force

# ---------- Stage 4: runtime ----------
FROM node:20-alpine AS runner
WORKDIR /app
ENV NODE_ENV=production

# Run as non-root
RUN addgroup -S app && adduser -S app -G app

COPY --from=prod-deps --chown=app:app /app/node_modules ./node_modules
COPY --from=build     --chown=app:app /app/dist ./dist
COPY --chown=app:app package.json ./

USER app
EXPOSE 3000

HEALTHCHECK --interval=30s --timeout=3s --start-period=10s --retries=3 \
  CMD wget -qO- http://localhost:3000/health || exit 1

CMD ["node", "dist/index.js"]
```

**Why multi-stage?** Final image has no source code, no dev dependencies, no compilers → smaller and safer.

### 7.2 `.dockerignore`

```
node_modules
dist
.git
.gitignore
.env
.env.*
coverage
*.md
Dockerfile
docker-compose*.yml
.husky
.vscode
```

### 7.3 Best practices checklist

- ✅ Pin base image versions (`node:20.17-alpine`, not `latest`)
- ✅ Non-root user
- ✅ Multi-stage builds
- ✅ Order layers so rarely-changing steps come first (better cache)
- ✅ Never bake secrets into images
- ✅ One process per container
- ✅ `HEALTHCHECK` defined
- ✅ Scan images (`trivy image myapp:tag`)
- ✅ Tag with **commit SHA** (immutable) in addition to `latest`/branch tags

### 7.4 `docker-compose.prod.yml`

```yaml
services:
  app:
    image: ${IMAGE}:${TAG}
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
    deploy:
      resources:
        limits:
          cpus: "1.0"
          memory: 512M
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
    depends_on: [app]
    networks: [internal]

volumes:
  pgdata:
  caddy_data:

networks:
  internal:
```

`Caddyfile` (automatic HTTPS):

```
example.com {
    encode gzip
    reverse_proxy app:3000
    header {
        Strict-Transport-Security "max-age=31536000; includeSubDomains"
        X-Content-Type-Options nosniff
        X-Frame-Options DENY
        Referrer-Policy strict-origin-when-cross-origin
    }
}
```

### 7.5 Useful Docker commands

```bash
docker build -t myapp:local .
docker run --rm -p 3000:3000 --env-file .env myapp:local
docker ps
docker logs -f --tail 100 <container>
docker exec -it <container> sh
docker stats
docker image ls && docker image prune -f
docker system df
docker compose -f docker-compose.prod.yml up -d
docker compose -f docker-compose.prod.yml ps
docker compose -f docker-compose.prod.yml logs -f app
```

---

## 8. Deployment to Staging and Production

### 8.1 Environments

| | Test | Staging | Production |
|---|---|---|---|
| Branch | `test` | `stage` | `main` |
| Purpose | Integration | Production rehearsal / QA | Real users |
| Data | Fake | Anonymized copy / seed | Real |
| Config | Debug-friendly | **Same as production** | Locked down |
| Deploy | Auto | Auto | Manual approval |
| URL | test.example.com | staging.example.com | example.com |

> **Staging must mirror production** (same OS, Docker version, DB version, reverse proxy). Otherwise it is useless.

### 8.2 Server preparation (Ubuntu VPS)

```bash
# 1. Create a non-root deploy user
adduser deploy
usermod -aG docker deploy
mkdir -p /home/deploy/.ssh
# paste the CI public key into /home/deploy/.ssh/authorized_keys

# 2. Harden SSH  (/etc/ssh/sshd_config)
#    PermitRootLogin no
#    PasswordAuthentication no
systemctl restart ssh

# 3. Firewall
ufw default deny incoming
ufw default allow outgoing
ufw allow OpenSSH
ufw allow 80
ufw allow 443
ufw enable

# 4. Brute-force protection + auto security updates
apt install -y fail2ban unattended-upgrades

# 5. Install Docker
curl -fsSL https://get.docker.com | sh

# 6. App directory
mkdir -p /opt/app && chown deploy:deploy /opt/app
```

### 8.3 `/opt/app/deploy.sh` (with automatic rollback)

```bash
#!/usr/bin/env bash
set -euo pipefail

APP_DIR=/opt/app
COMPOSE="docker compose -f $APP_DIR/docker-compose.prod.yml"
STATE_FILE=$APP_DIR/.current_tag
HEALTH_URL="http://localhost:3000/health"   # or via docker exec / proxy

cd "$APP_DIR"

PREVIOUS_TAG=$(cat "$STATE_FILE" 2>/dev/null || echo "none")
echo ">> Previous tag: $PREVIOUS_TAG"
echo ">> Deploying tag: $TAG"

export IMAGE TAG

# Pull new image first (fail early if missing)
docker pull "$IMAGE:$TAG"

# Start new version
$COMPOSE up -d --remove-orphans

# Health check loop (up to ~60s)
for i in $(seq 1 30); do
  if $COMPOSE exec -T app wget -qO- http://localhost:3000/health >/dev/null 2>&1; then
    echo ">> Healthy"
    echo "$TAG" > "$STATE_FILE"
    docker image prune -f >/dev/null
    exit 0
  fi
  sleep 2
done

echo "!! Health check failed. Rolling back to $PREVIOUS_TAG"
if [ "$PREVIOUS_TAG" != "none" ]; then
  export TAG=$PREVIOUS_TAG
  $COMPOSE up -d
fi
exit 1
```

### 8.4 Manual rollback

Because every image is tagged with a commit SHA, rollback = redeploy an older tag:

```bash
ssh deploy@server
cd /opt/app
cat .current_tag                       # current
IMAGE=registry.gitlab.com/you/app TAG=<old-sha> ./deploy.sh
```

Or in GitLab: **Deployments → Environments → production → Rollback** on a previous deployment.

> Database migrations complicate rollback. Write **backward-compatible migrations** (add columns first, remove them in a later release) so old code works with the new schema.

### 8.5 Release strategies (know these exist)

| Strategy | Idea | Downtime | Complexity |
|---|---|---|---|
| Recreate | Stop old, start new | Short | Low |
| Rolling | Replace instances gradually | None | Medium |
| Blue/Green | Run two stacks, switch traffic | None | Medium-High |
| Canary | Send 5% of traffic to new version first | None | High |

Start with Recreate (above), then learn Blue/Green.

---

## 9. Production Readiness

### 9.1 Environment variables and secrets

**Rules**

1. Config comes from the environment (12-factor app). Never hard-code.
2. **Never commit** `.env` files. Commit `.env.example` with fake values.
3. Validate env at startup, so the app fails fast.
4. Different secrets per environment. Staging must never share production credentials.
5. Store secrets in CI variables (masked + protected), or a secrets manager (Vault, AWS Secrets Manager, Doppler, SOPS).
6. Rotate secrets periodically and after any suspected leak.

**Validation example (zod):**

```ts
import { z } from 'zod';

const Env = z.object({
  NODE_ENV: z.enum(['development', 'test', 'production']),
  PORT: z.coerce.number().default(3000),
  DATABASE_URL: z.string().url(),
  JWT_SECRET: z.string().min(32),
  LOG_LEVEL: z.enum(['debug', 'info', 'warn', 'error']).default('info'),
});

export const env = Env.parse(process.env);   // throws on startup if invalid
```

**Secret scanning:** add `gitleaks` to pre-commit/CI.

```bash
gitleaks detect --source . --verbose
```

### 9.2 Security checklist

**Application**
- [ ] `helmet` (security headers), CORS restricted to known origins
- [ ] Rate limiting on auth and public endpoints
- [ ] Input validation on every endpoint (zod/joi)
- [ ] Parameterized queries / ORM (no string-built SQL)
- [ ] Passwords hashed with argon2/bcrypt
- [ ] Authorization checks on every resource, not just authentication
- [ ] Cookies: `HttpOnly`, `Secure`, `SameSite`
- [ ] Dependency audit in CI (`npm audit`, Dependabot/Renovate)

**Infrastructure**
- [ ] HTTPS everywhere (HSTS)
- [ ] Containers run non-root; read-only filesystem where possible
- [ ] Database not exposed publicly (internal Docker network only)
- [ ] Firewall: only 22/80/443 open
- [ ] SSH key-only login, root login disabled, fail2ban on
- [ ] Image vulnerability scanning (Trivy) in CI
- [ ] Automatic OS security updates
- [ ] Backups, and *tested restores*

**Process**
- [ ] Least privilege for CI tokens and deploy users
- [ ] Protected variables only available on protected branches
- [ ] Audit who can deploy to production

### 9.3 Logging

- Log **structured JSON** to stdout/stderr, and let Docker/an agent collect it.
- Include request ID, user ID (not PII), route, status, duration.
- Never log passwords, tokens, or full card/personal data.

```ts
import pino from 'pino';

export const logger = pino({
  level: process.env.LOG_LEVEL ?? 'info',
  redact: ['req.headers.authorization', 'password', 'token'],
});
```

**Log levels:** `error` (needs attention) → `warn` → `info` (business events) → `debug` (dev only).

**Centralization options:** Grafana Loki + Promtail, ELK/OpenSearch, Datadog, Better Stack.

### 9.4 Monitoring and alerting

Monitor the **four golden signals**: latency, traffic, errors, saturation.

| Need | Tool |
|---|---|
| Uptime checks | Uptime Kuma (self-hosted), UptimeRobot, Better Stack |
| Error tracking | **Sentry** |
| Metrics | Prometheus + Grafana, `prom-client` in Node |
| Host metrics | node_exporter, cAdvisor, Netdata |
| Alerts | Grafana Alerting, Alertmanager → Slack/Email/PagerDuty |

**Health endpoints:**

```ts
// Liveness: is the process up?
app.get('/health', (_req, res) => res.status(200).json({ status: 'ok' }));

// Readiness: can it serve traffic? (DB, cache reachable)
app.get('/ready', async (_req, res) => {
  try {
    await db.query('SELECT 1');
    res.status(200).json({ status: 'ready' });
  } catch {
    res.status(503).json({ status: 'not-ready' });
  }
});
```

**Alert on symptoms**, not noise: error rate > 2% for 5 min, p95 latency > 1s, disk > 85%, memory > 90%, container restarts, SSL expiring in < 14 days.

### 9.5 Graceful shutdown

Docker sends `SIGTERM` on stop/redeploy. Handle it or requests get dropped.

```ts
const server = app.listen(port);

const shutdown = (signal: string) => {
  logger.info({ signal }, 'shutting down');
  server.close(async () => {
    await db.end();
    process.exit(0);
  });
  setTimeout(() => process.exit(1), 10_000).unref();   // force exit
};

process.on('SIGTERM', () => shutdown('SIGTERM'));
process.on('SIGINT', () => shutdown('SIGINT'));
```

### 9.6 Backups and disaster recovery

```bash
# Nightly Postgres backup (cron)
0 2 * * * docker exec db pg_dump -U $USER $DB | gzip > /backups/db-$(date +\%F).sql.gz
# + copy off-server (S3, Backblaze, rsync) and prune old backups
```

- Follow **3-2-1**: 3 copies, 2 media types, 1 off-site.
- **A backup you haven't restored is a hope, not a backup.** Practice restores.
- Define **RPO** (how much data loss is OK) and **RTO** (how fast you must recover).

### 9.7 Server management basics

```bash
# Health of the box
htop                       # CPU / memory
df -h                      # disk
free -m                    # RAM
journalctl -u docker -f    # Docker daemon logs
ss -tulpn                  # listening ports
last                       # recent logins
ufw status verbose
systemctl status docker

# Maintenance
apt update && apt upgrade -y
docker system prune -af --volumes   # careful: removes unused volumes too!
```

Set up **log rotation** (Docker `max-size`, `logrotate`), **disk alerts**, and **automatic certificate renewal** (Caddy/Let's Encrypt handles it).

### 9.8 Versioning and releases

- Use **Semantic Versioning**: `MAJOR.MINOR.PATCH`.
- Automate with `release-please` or `semantic-release`, which reads Conventional Commits, bumps version, writes `CHANGELOG.md`, and creates tags.
- Tag releases on `main`: `v1.4.2`.

### 9.9 Production-readiness checklist (final)

- [ ] All config via env vars; nothing secret in Git or in images
- [ ] Health + readiness endpoints
- [ ] Graceful shutdown
- [ ] Structured logs with redaction
- [ ] Error tracking (Sentry) and uptime monitoring with alerts
- [ ] HTTPS + security headers
- [ ] Rate limiting + input validation
- [ ] Non-root, minimal, scanned Docker image
- [ ] CI blocks merges on failure
- [ ] Protected branches + required approvals
- [ ] Automated deploy to staging; approved deploy to production
- [ ] Tested rollback procedure
- [ ] Automated, tested backups
- [ ] Runbook: "what to do when X breaks"
- [ ] README documents setup, deploy, rollback

---

## 10. Learning Roadmap

| Phase | Topics | Outcome |
|---|---|---|
| **1. Git fundamentals** | Branching, rebase, merge, stash, cherry-pick, reflog, resolving conflicts | Comfortable with history |
| **2. Team workflow** | Branch strategy, conventional commits, MRs, reviews | Professional collaboration |
| **3. Local gates** | ESLint, Prettier, tsc, Husky, lint-staged, commitlint | Bad code can't be committed |
| **4. CI** | GitLab CI jobs, cache, artifacts, rules | Automatic verification |
| **5. Protection** | Protected branches, approvals, CODEOWNERS | No unreviewed code reaches `main` |
| **6. Docker** | Dockerfile, multi-stage, compose, networks, volumes | Reproducible builds |
| **7. Servers** | Linux, SSH, firewall, users, reverse proxy, TLS | Manage a real server |
| **8. CD** | Registry, SSH deploy, environments, manual gates | One-click releases |
| **9. Operations** | Logs, metrics, alerts, backups, rollback | Sleep well at night |
| **10. Advanced** | Terraform, Kubernetes, blue/green, canary, IaC | Scale up |

---

## 11. 10 Practice Tasks

Do these in order. Each builds on the previous. Use one small sample app (e.g. a TypeScript Express API with `/health`, `/users`) as your playground.

---

### Task 1: Git Branching Foundations

**Goal:** Build the `test → stage → main` structure and practice moving code through it.

**Steps**
1. Create a new GitLab project with a simple TypeScript API.
2. Create branches `test` and `stage` from `main`. Set `test` as the default branch.
3. Create `feat/add-health-endpoint` from `test`, make 3 commits, push.
4. Open an MR into `test` and merge it (squash).
5. Open an MR `test → stage`, merge. Then `stage → main`, merge.
6. Create a deliberate **merge conflict**: two branches edit the same line, then resolve it.
7. Practice: `git stash`, `git cherry-pick`, `git rebase -i` (squash 3 commits into 1), `git reflog` (recover a deleted branch).

**Done when:** you can explain when to use merge vs rebase vs squash, and `main` contains the code that went through all three branches.

---

### Task 2: Conventional Commits with commitlint

**Goal:** Reject badly-written commit messages automatically.

**Steps**
1. Install `husky` and `@commitlint/cli` + `@commitlint/config-conventional`.
2. Add the `commit-msg` hook.
3. Try `git commit -m "fixed stuff"`, which should **fail**.
4. Try `git commit -m "fix(api): handle empty request body"`, which should **pass**.
5. Make one commit of each type: `feat`, `fix`, `refactor`, `chore`, `docs`, `test`.
6. Add a breaking change commit using `!` and a `BREAKING CHANGE:` footer.
7. Add a custom rule (e.g. max subject length 72) and prove it works.

**Stretch:** install `commitizen` so `npm run commit` gives an interactive prompt.

**Done when:** it's impossible to commit a non-conventional message without `--no-verify`.

---

### Task 3: Pre-commit Quality Gate

**Goal:** Only clean, formatted, type-safe code can be committed.

**Steps**
1. Configure ESLint (flat config), Prettier, and `tsc --noEmit`.
2. Add `lint-staged` and a `pre-commit` hook running ESLint, Prettier, and typecheck.
3. Intentionally break things one at a time and confirm each is caught:
   - an unused variable
   - a missing semicolon / bad formatting (should be auto-fixed)
   - `const x: number = "hello"`
   - use of `any` (warning → error with `--max-warnings=0`)
4. Measure how long the hook takes. If > 10 seconds, move `typecheck` to pre-push.
5. Add `gitleaks` to the pre-commit hook and try committing a fake `AWS_SECRET=...`.

**Done when:** a commit with any of the above problems is rejected or auto-fixed.

---

### Task 4: Pre-push Gate with Tests and Build

**Goal:** Broken code can't leave your machine.

**Steps**
1. Add Vitest and write at least 5 unit tests (e.g. a `calculateTotal()` function and a `/health` route test with `supertest`).
2. Add a `pre-push` hook running lint, typecheck, test, build.
3. Break a test on purpose, then try `git push` and confirm it is **blocked**.
4. Break the build (a TypeScript error in `src/`), then confirm it is blocked.
5. Add a coverage threshold (e.g. 70%) in `vitest.config.ts` and verify the hook fails when coverage drops.
6. Bypass with `git push --no-verify`, then explain in one paragraph **why this proves CI is still necessary** (answered in Task 5).

**Done when:** a failing test or failing build always blocks the push.

---

### Task 5: Build a CI Pipeline

**Goal:** The server independently verifies everything.

**Steps**
1. Create `.gitlab-ci.yml` with stages `quality → test → build`.
2. Make `lint`, `format`, `typecheck` run **in parallel**.
3. Add dependency caching keyed on `package-lock.json`, and compare pipeline time before/after.
4. Publish test coverage and view it in the MR.
5. Add `npm audit` as an allowed-failure job.
6. Push a commit using `--no-verify` that has a lint error. **The pipeline must go red.**
7. Fix it and watch the pipeline go green.
8. Make the pipeline run only for MRs and for `test`/`stage`/`main` (use `workflow:rules`).

**Stretch:** add a `matrix` job to test on Node 20 and 22.

**Done when:** a red pipeline appears for any quality failure, regardless of local hooks.

---

### Task 6: Branch Protection and Review Process

**Goal:** Enforce who can merge what, and make reviews mandatory.

**Steps**
1. Protect `main` and `stage`: no direct pushes, no force pushes; only Maintainers can merge.
2. Protect `test`: Developers can merge via MR only.
3. Require: pipeline must pass, all threads resolved, author can't self-approve.
4. Set approvals: 1 for `test`, 2 for `main`.
5. Add `CODEOWNERS` so changes to `Dockerfile` and `.gitlab-ci.yml` require a specific owner.
6. Add an MR template with a checklist.
7. With a second account (or a teammate), do a full review cycle:
   - open an MR with an intentional bug and a hard-coded secret
   - the reviewer leaves comments, requests changes
   - you fix, push, get approval, merge
8. Try to push directly to `main`. It must be rejected. Screenshot the error.

**Done when:** no code can reach `stage` or `main` without a green pipeline and the right approvals.

---

### Task 7: Dockerize the Application

**Goal:** Build a small, secure, production-grade image.

**Steps**
1. Write a naive single-stage Dockerfile. Record the image size (`docker image ls`).
2. Rewrite it as a **multi-stage** build with a non-root user. Compare sizes.
3. Add `.dockerignore` and observe the build-context size shrink.
4. Add a `HEALTHCHECK` and verify `docker ps` shows `(healthy)`.
5. Reorder layers and prove caching works (change source only, so `npm ci` should be cached).
6. Scan with Trivy: `trivy image myapp:local`. Fix or document one finding.
7. Verify the container runs as non-root: `docker exec <id> whoami`.
8. Verify no secrets or `.env` are inside: `docker run --rm myapp:local ls -la /app`.
9. Write `docker-compose.yml` with app + Postgres + health-gated `depends_on`.

**Done when:** image < ~200MB (for a typical Node API), non-root, healthy, scanned.

---

### Task 8: Containerize the CI Build and Use a Registry

**Goal:** CI builds and publishes versioned images.

**Steps**
1. Add a `build-image` job (Docker-in-Docker or Kaniko) to the pipeline.
2. Push images to the GitLab Container Registry, tagged with:
   - `$CI_COMMIT_SHORT_SHA` (immutable)
   - branch name (`stage`, `main`)
3. Run the image on your laptop pulled **from the registry**:
   `docker run -p 3000:3000 registry.gitlab.com/you/app:<sha>`
4. Add a Trivy scan job that fails on `CRITICAL` vulnerabilities.
5. Add a **registry cleanup policy** to delete old tags.
6. Add `release-please` (or `semantic-release`) so merging to `main` generates a version tag (`v1.2.0`) and `CHANGELOG.md` from your conventional commits.

**Done when:** every merge to `stage`/`main` produces a scanned, SHA-tagged image in the registry, and `main` produces a versioned release.

---

### Task 9: Provision a Server and Deploy to Staging

**Goal:** Fully automated staging deployment on a real server.

**Steps**
1. Rent a cheap VPS (any provider, a ~$5 plan is fine) or use a local VM (Multipass/VirtualBox).
2. Harden it:
   - non-root `deploy` user in the `docker` group
   - SSH key-only login, root login disabled
   - `ufw` allowing only 22/80/443
   - `fail2ban` and `unattended-upgrades`
3. Install Docker. Create `/opt/app` with `docker-compose.prod.yml`, `Caddyfile`, and `.env.production` (created manually on the server and never in Git).
4. Point a DNS record (`staging.yourdomain.com`) to the server. Caddy should get an HTTPS certificate automatically.
5. Generate an SSH key pair for CI. Put the public key on the server; store the private key as a **masked, protected** CI variable.
6. Write `deploy.sh` (see section 8.3).
7. Add `deploy-staging` job that runs on the `stage` branch.
8. Merge an MR into `stage` and watch the app deploy itself. Visit `https://staging.yourdomain.com/health`.
9. Break the app on purpose (e.g. crash on startup) and confirm the **health check fails and auto-rollback** restores the previous version.

**Done when:** merging into `stage` deploys to a live HTTPS URL with no manual steps, and a bad release rolls itself back.

---

### Task 10: Production Release, Observability, and a Disaster Drill

**Goal:** Ship to production with approvals, monitoring, backups, and a rehearsed rollback.

**Steps**

**A. Production deploy**
1. Create a second environment (second VPS, or a separate compose project/port) for production.
2. Add `deploy-production` job on `main` with `when: manual`, and make `production` a **Protected Environment** (only Maintainers can deploy).
3. Use **different** secrets and a different database than staging.

**B. Observability**
4. Switch the app to structured JSON logging (`pino`) with secret redaction.
5. Add Sentry; trigger a test error and see it appear.
6. Add Uptime Kuma (or UptimeRobot) hitting `/health` every minute, with Slack/Email alerts.
7. Expose Prometheus metrics (`prom-client`) and build one Grafana dashboard (request rate, error rate, p95 latency).

**C. Reliability**
8. Implement graceful shutdown (`SIGTERM`) and verify no requests are dropped during a redeploy (run a load test with `autocannon` or `k6` while deploying).
9. Set up a nightly DB backup via cron, copied off-server.

**D. The drills (do not skip these)**
10. **Rollback drill:** deploy a bad version to production, then roll back to the previous SHA in under 2 minutes.
11. **Hotfix drill:** simulate a production bug → `hotfix/*` from `main` → MR → approvals → deploy → back-merge to `stage` and `test`.
12. **Restore drill:** delete the database in staging and restore from backup. Time it.
13. Write a one-page **runbook** covering: how to deploy, how to roll back, where logs are, who to contact, how to restore a backup.

**Done when:** you can deploy, observe, break, roll back, hotfix, and restore, calmly and from documentation.

---

### Bonus Challenges (after the 10)

- Replace SSH deploy with **Ansible** or **Terraform + cloud provider**.
- Implement **blue/green deployment** with Caddy/Nginx switching upstreams.
- Add **Renovate/Dependabot** for automated dependency MRs.
- Add **preview environments** per MR (`review/*` environments in GitLab).
- Add **E2E tests** (Playwright) as a CI stage against staging.
- Migrate to **Kubernetes** (k3s) with Helm.

---

## 12. Cheat Sheet

### Git

```bash
git switch -c feat/x test            # new branch from test
git commit -m "feat(x): ..."
git fetch origin && git rebase origin/test
git push -u origin feat/x
git log --oneline --graph --decorate --all
git reflog                           # recover "lost" commits
git revert <sha>                     # safe undo (new commit)
git restore --staged <file>          # unstage
git stash push -m "wip" && git stash pop
git cherry-pick <sha>
git rebase -i HEAD~3                 # squash / reword last 3
```

### Hooks summary

| Hook | Runs | Checks |
|---|---|---|
| `pre-commit` | each commit | lint-staged (ESLint + Prettier), typecheck, gitleaks |
| `commit-msg` | each commit | commitlint |
| `pre-push` | each push | lint, typecheck, test, build |

### Pipeline summary

| Branch | Quality | Test | Build image | Deploy |
|---|---|---|---|---|
| MR / feature | ✅ | ✅ | – | – |
| `test` | ✅ | ✅ | – | optional |
| `stage` | ✅ | ✅ | ✅ | auto → staging |
| `main` | ✅ | ✅ | ✅ | manual → production |

### Golden rules

1. Never commit directly to `stage` or `main`.
2. Never commit secrets. Rotate immediately if you do.
3. Local hooks help; **CI enforces**.
4. Build once, deploy the **same image** everywhere. Only config differs.
5. Tag images with commit SHAs. Never rely on `latest` for deploys.
6. Staging should mirror production.
7. Every deploy needs a tested rollback.
8. Untested backups don't exist.
9. If you can't see it (logs, metrics, alerts), you can't operate it.
10. Keep MRs small and reviews honest.

---

*Suggested pacing: Tasks 1–4 (week 1), Tasks 5–6 (week 2), Task 7–8 (week 3), Task 9 (week 4), Task 10 (week 5–6).*

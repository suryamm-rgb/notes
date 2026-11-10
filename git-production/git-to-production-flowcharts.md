# Git → CI/CD → Docker → Production: Flowcharts

> All diagrams use **Mermaid**, which renders automatically in GitLab, GitHub, VS Code (with the "Markdown Preview Mermaid Support" extension), Obsidian, and Notion.
> If you view this file in a plain text editor, paste any diagram into https://mermaid.live to see it.

## Contents

1. [End-to-end overview](#1-end-to-end-overview)
2. [Branching strategy: test → stage → main](#2-branching-strategy-test--stage--main)
3. [Daily developer workflow](#3-daily-developer-workflow)
4. [Local gates: commit and push hooks](#4-local-gates-commit-and-push-hooks)
5. [CI pipeline](#5-ci-pipeline)
6. [Merge Request, review and protection](#6-merge-request-review-and-protection)
7. [Docker image build](#7-docker-image-build)
8. [Deployment with automatic rollback](#8-deployment-with-automatic-rollback)
9. [Deploy sequence: who talks to whom](#9-deploy-sequence-who-talks-to-whom)
10. [Hotfix flow](#10-hotfix-flow)
11. [Production monitoring and incident response](#11-production-monitoring-and-incident-response)
12. [Server architecture](#12-server-architecture)
13. [Learning roadmap](#13-learning-roadmap)

---

## 1. End-to-end overview

The full journey of one change, from your keyboard to production.

```mermaid
flowchart TD
    A["Developer writes code on feature branch"] --> B["git commit"]
    B --> C{"Pre-commit hook passes? ESLint, Prettier, tsc, commitlint"}
    C -- "No" --> B
    C -- "Yes" --> D["git push"]
    D --> E{"Pre-push hook passes? lint, types, tests, build"}
    E -- "No" --> A
    E -- "Yes" --> F["Open Merge Request into test"]
    F --> G{"CI pipeline green?"}
    G -- "No" --> A
    G -- "Yes" --> H{"Reviewer approves?"}
    H -- "Changes requested" --> A
    H -- "Approved" --> I["Merge into test"]
    I --> J["MR: test into stage"]
    J --> K{"CI green and approved?"}
    K -- "No" --> A
    K -- "Yes" --> L["Merge into stage"]
    L --> M["Build Docker image and push to registry"]
    M --> N["Auto deploy to STAGING"]
    N --> O{"QA and smoke tests pass?"}
    O -- "No" --> A
    O -- "Yes" --> P["MR: stage into main"]
    P --> Q{"CI green and 2 approvals?"}
    Q -- "No" --> A
    Q -- "Yes" --> R["Merge into main"]
    R --> S["Build and tag release image"]
    S --> T{"Manual approval to deploy"}
    T -- "Approved" --> U["Deploy to PRODUCTION"]
    U --> V{"Health check OK?"}
    V -- "Yes" --> W["Release complete. Monitor."]
    V -- "No" --> X["Automatic rollback to previous version"]
    X --> A

    style A fill:#e0f2fe,stroke:#0284c7
    style W fill:#dcfce7,stroke:#16a34a
    style X fill:#fee2e2,stroke:#dc2626
    style U fill:#fef9c3,stroke:#ca8a04
```

---

## 2. Branching strategy: test → stage → main

```mermaid
gitGraph
    commit id: "initial"
    branch stage
    branch test
    checkout test
    branch feat/login
    commit id: "feat: add login form"
    commit id: "feat: add validation"
    checkout test
    merge feat/login id: "squash merge"
    branch fix/cart
    commit id: "fix: cart total"
    checkout test
    merge fix/cart id: "squash merge "
    checkout stage
    merge test id: "promote to stage"
    checkout main
    merge stage id: "release v1.0.0" tag: "v1.0.0"
```

Branch roles at a glance:

```mermaid
flowchart LR
    F1["feat/*"] --> T["test<br/>Integration"]
    F2["fix/*"] --> T
    F3["refactor/*"] --> T
    F4["chore/*"] --> T
    T -- "MR + 1 approval" --> S["stage<br/>Staging server<br/>QA / UAT"]
    S -- "MR + 2 approvals" --> M["main<br/>Production"]

    style T fill:#dbeafe,stroke:#2563eb
    style S fill:#fef9c3,stroke:#ca8a04
    style M fill:#dcfce7,stroke:#16a34a
```

---

## 3. Daily developer workflow

```mermaid
flowchart TD
    A["git switch test"] --> B["git pull origin test"]
    B --> C["git switch -c feat/my-feature"]
    C --> D["Write code and tests"]
    D --> E["git add ."]
    E --> F["git commit -m 'feat(scope): message'"]
    F --> G{"Hooks pass?"}
    G -- "No" --> H["Fix issues"]
    H --> E
    G -- "Yes" --> I{"More work?"}
    I -- "Yes" --> D
    I -- "No" --> J["git fetch and git rebase origin/test"]
    J --> K{"Conflicts?"}
    K -- "Yes" --> L["Resolve, git rebase --continue"]
    L --> M["git push -u origin feat/my-feature"]
    K -- "No" --> M
    M --> N["Open Merge Request into test"]
    N --> O["Address review comments"]
    O --> P["Squash merge and delete branch"]
    P --> A

    style P fill:#dcfce7,stroke:#16a34a
```

---

## 4. Local gates: commit and push hooks

```mermaid
flowchart TD
    subgraph COMMIT["git commit"]
        direction TB
        C1["pre-commit hook"] --> C2["lint-staged: ESLint --fix on staged files"]
        C2 --> C3["Prettier --write on staged files"]
        C3 --> C4["tsc --noEmit type check"]
        C4 --> C5["gitleaks: secret scan"]
        C5 --> C6["commit-msg hook: commitlint"]
    end

    subgraph PUSH["git push"]
        direction TB
        P1["pre-push hook"] --> P2["npm run lint"]
        P2 --> P3["npm run typecheck"]
        P3 --> P4["npm run test"]
        P4 --> P5["npm run build"]
    end

    START["Developer runs git commit"] --> C1
    C6 -- "All pass" --> OK1["Commit created"]
    C2 -- "Fail" --> FAIL1["Commit rejected"]
    C4 -- "Fail" --> FAIL1
    C5 -- "Fail" --> FAIL1
    C6 -- "Bad message" --> FAIL1

    OK1 --> START2["Developer runs git push"]
    START2 --> P1
    P5 -- "All pass" --> OK2["Push sent to remote"]
    P2 -- "Fail" --> FAIL2["Push rejected"]
    P3 -- "Fail" --> FAIL2
    P4 -- "Fail" --> FAIL2
    P5 -- "Fail" --> FAIL2

    OK2 --> CI["CI repeats every check on the server"]

    style FAIL1 fill:#fee2e2,stroke:#dc2626
    style FAIL2 fill:#fee2e2,stroke:#dc2626
    style OK1 fill:#dcfce7,stroke:#16a34a
    style OK2 fill:#dcfce7,stroke:#16a34a
    style CI fill:#e0f2fe,stroke:#0284c7
```

> Hooks can be bypassed with `--no-verify`. CI cannot. That is why both exist.

---

## 5. CI pipeline

```mermaid
flowchart LR
    TRIGGER["Push or Merge Request"] --> Q

    subgraph Q["Stage 1: quality (parallel)"]
        direction TB
        Q1["lint"]
        Q2["format:check"]
        Q3["typecheck"]
        Q4["npm audit"]
    end

    Q --> T

    subgraph T["Stage 2: test"]
        direction TB
        T1["unit tests"]
        T2["integration tests"]
        T3["coverage report"]
    end

    T --> B

    subgraph B["Stage 3: build"]
        direction TB
        B1["npm run build"]
        B2["docker build"]
        B3["Trivy image scan"]
        B4["push to registry with SHA tag"]
    end

    B --> D{"Which branch?"}
    D -- "feature / MR" --> STOP["Stop: no deploy"]
    D -- "test" --> DT["Optional deploy to test env"]
    D -- "stage" --> DS["Auto deploy to STAGING"]
    D -- "main" --> DM["Manual gate then PRODUCTION"]

    style STOP fill:#f3f4f6,stroke:#6b7280
    style DS fill:#fef9c3,stroke:#ca8a04
    style DM fill:#dcfce7,stroke:#16a34a
```

---

## 6. Merge Request, review and protection

```mermaid
flowchart TD
    A["Author opens Merge Request"] --> B["MR template checklist filled"]
    B --> C["CI pipeline starts automatically"]
    C --> D{"Pipeline green?"}
    D -- "No" --> E["Author fixes and pushes"]
    E --> C
    D -- "Yes" --> F["CODEOWNERS auto-assigned"]
    F --> G["Reviewer checks: correctness, security, tests, readability, ops impact"]
    G --> H{"Decision"}
    H -- "Request changes" --> I["Author addresses comments"]
    I --> J["New commits remove old approvals"]
    J --> C
    H -- "Approve" --> K{"Required approvals reached?"}
    K -- "No" --> G
    K -- "Yes" --> L{"All threads resolved?"}
    L -- "No" --> I
    L -- "Yes" --> M{"Author is also approver?"}
    M -- "Yes" --> N["Blocked: author cannot self-approve"]
    N --> G
    M -- "No" --> O["Merge button enabled"]
    O --> P{"Is user allowed to merge into this branch?"}
    P -- "No" --> Q["Maintainer merges"]
    P -- "Yes" --> R["Merge"]
    Q --> S["Branch deleted, deploy pipeline triggered"]
    R --> S

    style N fill:#fee2e2,stroke:#dc2626
    style S fill:#dcfce7,stroke:#16a34a
```

Who can do what:

```mermaid
flowchart LR
    subgraph RULES["Protection rules"]
        direction TB
        R1["test: Developers merge via MR, 1 approval"]
        R2["stage: Maintainers merge, 1 to 2 approvals"]
        R3["main: Maintainers merge, 2 approvals, no direct push, no force push"]
    end
    DEV["Developer"] --> R1
    LEAD["Tech lead"] --> R2
    MAINT["Maintainer"] --> R3
```

---

## 7. Docker image build

```mermaid
flowchart TD
    SRC["Source code and package.json"] --> S1

    subgraph S1["Stage 1: deps"]
        D1["npm ci: all dependencies"]
    end

    subgraph S2["Stage 2: build"]
        D2["Copy node_modules and source"]
        D3["npm run build: produces dist"]
        D2 --> D3
    end

    subgraph S3["Stage 3: prod-deps"]
        D4["npm ci --omit=dev"]
    end

    subgraph S4["Stage 4: runner - final image"]
        D5["Create non-root user"]
        D6["Copy prod node_modules"]
        D7["Copy dist only"]
        D8["HEALTHCHECK, USER app, CMD node dist/index.js"]
        D5 --> D6 --> D7 --> D8
    end

    S1 --> S2
    S1 -.-> S3
    S2 --> S4
    S3 --> S4
    S4 --> SCAN{"Trivy scan: CRITICAL found?"}
    SCAN -- "Yes" --> FAIL["Fail pipeline"]
    SCAN -- "No" --> TAG["Tag with commit SHA and branch name"]
    TAG --> PUSH["Push to container registry"]

    style FAIL fill:#fee2e2,stroke:#dc2626
    style PUSH fill:#dcfce7,stroke:#16a34a
    style S4 fill:#eff6ff,stroke:#2563eb
```

Why multi-stage: the final image has no source code, no dev dependencies, no compilers. It is smaller and has fewer vulnerabilities.

---

## 8. Deployment with automatic rollback

```mermaid
flowchart TD
    A["CI job starts deploy"] --> B["SSH into server as deploy user"]
    B --> C["Read .current_tag: PREVIOUS_TAG"]
    C --> D["docker pull image:NEW_TAG"]
    D --> E{"Pull succeeded?"}
    E -- "No" --> FAIL0["Abort: nothing changed"]
    E -- "Yes" --> F["docker compose up -d with NEW_TAG"]
    F --> G["Wait and poll /health"]
    G --> H{"Healthy within 60 seconds?"}
    H -- "Yes" --> I["Save NEW_TAG to .current_tag"]
    I --> J["Prune old images"]
    J --> K["Deploy successful"]
    H -- "No" --> L{"PREVIOUS_TAG exists?"}
    L -- "Yes" --> M["docker compose up -d with PREVIOUS_TAG"]
    M --> N["Rolled back. Pipeline marked failed. Alert sent."]
    L -- "No" --> O["Stop containers and alert on-call"]

    style K fill:#dcfce7,stroke:#16a34a
    style N fill:#fef9c3,stroke:#ca8a04
    style O fill:#fee2e2,stroke:#dc2626
    style FAIL0 fill:#f3f4f6,stroke:#6b7280
```

---

## 9. Deploy sequence: who talks to whom

```mermaid
sequenceDiagram
    autonumber
    actor Dev as Developer
    participant GL as GitLab
    participant Runner as CI Runner
    participant Reg as Container Registry
    participant Srv as Server (deploy.sh)
    participant App as App container
    participant Mon as Monitoring

    Dev->>GL: Merge MR into stage
    GL->>Runner: Trigger pipeline
    Runner->>Runner: lint, typecheck, test, build
    Runner->>Reg: Push image app:SHA
    Runner->>Srv: SSH: run deploy.sh with TAG=SHA
    Srv->>Reg: docker pull app:SHA
    Srv->>App: docker compose up -d
    loop Every 2s up to 60s
        Srv->>App: GET /health
        App-->>Srv: 200 OK or failure
    end
    alt Healthy
        Srv-->>Runner: Exit 0
        Runner-->>GL: Pipeline success
        GL-->>Dev: Notification: deployed
    else Unhealthy
        Srv->>App: Redeploy previous tag
        Srv-->>Runner: Exit 1
        Runner-->>GL: Pipeline failed
        GL-->>Dev: Notification: rolled back
    end
    App->>Mon: Logs, metrics, errors
    Mon-->>Dev: Alerts if thresholds are crossed
```

---

## 10. Hotfix flow

Production is broken and cannot wait for the normal `test → stage → main` promotion.

```mermaid
flowchart TD
    A["Production incident detected"] --> B["Create hotfix/issue-name from main"]
    B --> C["Write minimal fix and a regression test"]
    C --> D["Commit: fix(scope): message"]
    D --> E["Open MR into main"]
    E --> F{"CI green?"}
    F -- "No" --> C
    F -- "Yes" --> G{"Expedited review approved?"}
    G -- "No" --> C
    G -- "Yes" --> H["Merge into main"]
    H --> I["Manual deploy to PRODUCTION"]
    I --> J{"Problem resolved?"}
    J -- "No" --> K["Roll back and investigate"]
    K --> C
    J -- "Yes" --> L["Back-merge main into stage"]
    L --> M["Back-merge stage into test"]
    M --> N["Write post-incident notes"]

    style A fill:#fee2e2,stroke:#dc2626
    style N fill:#dcfce7,stroke:#16a34a
    style L fill:#fef9c3,stroke:#ca8a04
    style M fill:#fef9c3,stroke:#ca8a04
```

> **Do not skip the back-merge.** Otherwise the fix disappears in the next promotion from `test`.

---

## 11. Production monitoring and incident response

```mermaid
flowchart TD
    subgraph SIGNALS["What you collect"]
        L["Logs: JSON via pino"]
        M["Metrics: Prometheus"]
        E["Errors: Sentry"]
        U["Uptime: Uptime Kuma"]
    end

    L --> DASH["Dashboards: Grafana"]
    M --> DASH
    E --> ALERT
    U --> ALERT
    DASH --> ALERT{"Threshold crossed? error rate, latency, disk, memory, SSL expiry"}

    ALERT -- "No" --> OK["Healthy: keep watching"]
    ALERT -- "Yes" --> PAGE["Notify Slack, email or pager"]
    PAGE --> TRIAGE{"Caused by recent deploy?"}
    TRIAGE -- "Yes" --> RB["Roll back to previous SHA"]
    TRIAGE -- "No" --> INV["Investigate logs and metrics"]
    INV --> FIX{"Quick fix possible?"}
    FIX -- "Yes" --> HF["Hotfix flow"]
    FIX -- "No" --> MIT["Mitigate: scale, disable feature, maintenance page"]
    RB --> VERIFY["Verify health"]
    HF --> VERIFY
    MIT --> VERIFY
    VERIFY --> PM["Post-incident review and runbook update"]

    style OK fill:#dcfce7,stroke:#16a34a
    style PAGE fill:#fee2e2,stroke:#dc2626
    style PM fill:#e0f2fe,stroke:#0284c7
```

---

## 12. Server architecture

```mermaid
flowchart TB
    USER["Users / Browsers"] -- "HTTPS 443" --> FW

    subgraph SERVER["Linux VPS"]
        FW["Firewall ufw: 22, 80, 443 only"]
        subgraph DOCKER["Docker network: internal"]
            CADDY["Caddy reverse proxy: auto TLS, security headers"]
            APP["App container: Node.js, non-root, healthcheck"]
            DB[("PostgreSQL: volume pgdata")]
        end
        FW --> CADDY
        CADDY --> APP
        APP --> DB
        CRON["Cron: nightly pg_dump"] --> DB
        F2B["fail2ban and unattended-upgrades"]
    end

    CRON --> BK[("Off-site backup storage")]
    APP -. "logs and metrics" .-> OBS["Sentry, Grafana, Uptime Kuma"]
    CI["GitLab CI runner"] -- "SSH deploy user" --> FW
    REG["Container registry"] -. "docker pull" .-> APP

    style DB fill:#fef3c7,stroke:#d97706
    style BK fill:#fef3c7,stroke:#d97706
    style CADDY fill:#dbeafe,stroke:#2563eb
    style APP fill:#dcfce7,stroke:#16a34a
```

The database is on the internal network only and is never exposed to the internet.

---

## 13. Learning roadmap

```mermaid
flowchart LR
    P1["1. Git fundamentals<br/>branch, rebase, reflog"] --> P2["2. Team workflow<br/>branching, MRs, conventional commits"]
    P2 --> P3["3. Local gates<br/>ESLint, Prettier, tsc, Husky"]
    P3 --> P4["4. CI<br/>GitLab CI jobs, cache"]
    P4 --> P5["5. Protection<br/>protected branches, approvals"]
    P5 --> P6["6. Docker<br/>multi-stage, compose"]
    P6 --> P7["7. Servers<br/>Linux, SSH, firewall, TLS"]
    P7 --> P8["8. CD<br/>registry, deploy, environments"]
    P8 --> P9["9. Operations<br/>logs, metrics, backups, rollback"]
    P9 --> P10["10. Advanced<br/>Terraform, Kubernetes, blue/green"]

    style P1 fill:#e0f2fe,stroke:#0284c7
    style P5 fill:#fef9c3,stroke:#ca8a04
    style P9 fill:#dcfce7,stroke:#16a34a
    style P10 fill:#f3e8ff,stroke:#9333ea
```

Mapping to the 10 practice tasks:

```mermaid
flowchart TD
    T1["Task 1: Git branching"] --> T2["Task 2: Conventional commits"]
    T2 --> T3["Task 3: Pre-commit gate"]
    T3 --> T4["Task 4: Pre-push gate"]
    T4 --> T5["Task 5: CI pipeline"]
    T5 --> T6["Task 6: Branch protection and reviews"]
    T6 --> T7["Task 7: Dockerize app"]
    T7 --> T8["Task 8: Registry and releases"]
    T8 --> T9["Task 9: Server and staging deploy"]
    T9 --> T10["Task 10: Production, observability, drills"]

    T1:::phase1
    T2:::phase1
    T3:::phase2
    T4:::phase2
    T5:::phase3
    T6:::phase3
    T7:::phase4
    T8:::phase4
    T9:::phase5
    T10:::phase5

    classDef phase1 fill:#e0f2fe,stroke:#0284c7
    classDef phase2 fill:#ede9fe,stroke:#7c3aed
    classDef phase3 fill:#fef9c3,stroke:#ca8a04
    classDef phase4 fill:#ffedd5,stroke:#ea580c
    classDef phase5 fill:#dcfce7,stroke:#16a34a
```

---

## Quick legend

| Shape / color | Meaning |
|---|---|
| Diamond | A decision or gate |
| Rectangle | An action or step |
| Green | Success / finished state |
| Red | Failure / blocked / incident |
| Yellow | Caution, manual step, or approval |
| Blue | Information / start or tooling |

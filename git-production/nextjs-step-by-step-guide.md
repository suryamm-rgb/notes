# Next.js From Zero to Production-Ready: A Step-by-Step Beginner Guide

> **What you will build:** a small but real app called **TaskFlow**, a task manager with a home page, a tasks page, a task detail page, a REST API, health check, tests, quality hooks, CI, and a Docker image.
>
> **What you will learn:** installation, folder structure, routing, server vs client components, API routes, environment variables, testing, Git workflow (`test → stage → main`), pre-commit/pre-push hooks, Conventional Commits, CI, and Docker.
>
> **Versions used:** Next.js 16.x (App Router, Turbopack by default), React 19, TypeScript, Tailwind CSS v4, ESLint, Node.js 20.9 or newer (Node 22 recommended).

---

## How to read this guide

Every step uses the same format so you never have to guess where code goes:

- 💻 **Terminal** = type this in your terminal (inside the project folder unless told otherwise).
- 📄 **File: `path/to/file`** = open (or create) this file in your editor and paste the code.
- **Action** tells you whether to **create a new file** or **replace the whole content** of an existing one.

> **Tip:** Use VS Code. Create files with right-click → *New File* in the Explorer. Paths always start from the project root (the `taskflow` folder).

---

## Table of Contents

- [Part 1: Prerequisites](#part-1-prerequisites)
- [Part 2: Create the Next.js project](#part-2-create-the-nextjs-project)
- [Part 3: Understand what was generated](#part-3-understand-what-was-generated)
- [Part 4: Git setup with test, stage, main](#part-4-git-setup-with-test-stage-main)
- [Part 5: Quality tooling (Prettier, Vitest, Husky, commitlint)](#part-5-quality-tooling-prettier-vitest-husky-commitlint)
- [Part 6: Build the app](#part-6-build-the-app)
- [Part 7: Run and test everything](#part-7-run-and-test-everything)
- [Part 8: Environment variables](#part-8-environment-variables)
- [Part 9: Docker](#part-9-docker)
- [Part 10: CI pipeline](#part-10-ci-pipeline)
- [Part 11: Push and promote through branches](#part-11-push-and-promote-through-branches)
- [Troubleshooting](#troubleshooting)
- [Final folder structure](#final-folder-structure)
- [Next steps](#next-steps)

---

## Part 1: Prerequisites

You need three things: **Node.js**, **Git**, and a **code editor**.

### 1.1 Install Node.js (version 20.9 or newer)

Download the **LTS** version from https://nodejs.org, or use a version manager:

```bash
# macOS / Linux (nvm): install nvm from https://github.com/nvm-sh/nvm, then:
nvm install 22
nvm use 22
```

On Windows, you can use the installer from nodejs.org or [nvm-windows](https://github.com/coreybutler/nvm-windows).

### 1.2 Install Git

Download from https://git-scm.com, then tell Git who you are (one time only):

```bash
git config --global user.name "Your Name"
git config --global user.email "you@example.com"
git config --global init.defaultBranch main
```

### 1.3 Install VS Code (recommended)

https://code.visualstudio.com. Useful extensions: **ESLint**, **Prettier - Code formatter**, **Tailwind CSS IntelliSense**.

### 1.4 Verify everything

💻 **Terminal**

```bash
node --version     # should print v20.9.0 or higher (v22.x is ideal)
npm --version      # comes with Node
git --version
```

If `node --version` prints something lower than 20.9, update Node before continuing.

---

## Part 2: Create the Next.js project

### 2.1 Choose a folder and run the generator

💻 **Terminal**: go to the folder where you keep your projects, then run:

```bash
npx create-next-app@latest taskflow
```

`npx` downloads and runs the official generator. `taskflow` is the project name (it becomes the folder name).

### 2.2 Answer the prompts

The generator asks questions. Choose the options below. If it first asks **"Would you like to use the recommended Next.js defaults?"**, choose **"No, customize settings"** so you can pick the `src/` folder.

| Prompt | Your answer |
|---|---|
| Would you like to use TypeScript? | **Yes** |
| Which linter would you like to use? / Would you like to use ESLint? | **ESLint** (Yes) |
| Would you like to use Tailwind CSS? | **Yes** |
| Would you like your code inside a `src/` directory? | **Yes** |
| Would you like to use App Router? (recommended) | **Yes** |
| Would you like to use React Compiler? | **No** (keep it simple for now) |
| Would you like to customize the import alias (`@/*`)? | **No** (keep `@/*`) |
| Anything about `AGENTS.md` | Your choice. It is optional and does not affect the app. |

> **Prefer no questions?** This single command gives the same result:
>
> ```bash
> npx create-next-app@latest taskflow --ts --tailwind --eslint --app --src-dir --turbopack --import-alias "@/*" --use-npm
> ```
>
> Prompts change slightly between releases. If your screen looks a little different, just pick the equivalent of the table above: **TypeScript, ESLint, Tailwind, `src/` directory, App Router, alias `@/*`.**

The generator creates the folder, installs dependencies, and also runs `git init` with a first commit for you.

### 2.3 Enter the project and start the dev server

💻 **Terminal**

```bash
cd taskflow
npm run dev
```

Open **http://localhost:3000** in your browser. You should see the default Next.js welcome page. 🎉

Leave the server running while you code. Stop it any time with `Ctrl + C`. Open a **second terminal tab** for the other commands in this guide.

---

## Part 3: Understand what was generated

Open the project in VS Code (`code .`). The important parts:

```text
taskflow/
├── public/               Static files served as-is (images, icons). /logo.png → public/logo.png
├── src/
│   └── app/              The App Router: folders here become URLs
│       ├── favicon.ico
│       ├── globals.css   Global styles (Tailwind is imported here)
│       ├── layout.tsx    Root layout: wraps EVERY page (<html>, <body>)
│       └── page.tsx      The home page, URL: /
├── eslint.config.mjs     ESLint rules
├── next.config.ts        Next.js configuration
├── package.json          Dependencies and npm scripts
├── postcss.config.mjs    Tailwind/PostCSS wiring
├── tsconfig.json         TypeScript configuration
└── .gitignore            Files Git should never track
```

### The five rules of the App Router (memorize these)

1. **Folders = URLs.** `src/app/tasks/page.tsx` → `/tasks`.
2. **`page.tsx`** makes a folder a public page.
3. **`layout.tsx`** wraps pages (and nested pages) with shared UI like a navbar.
4. **`route.ts`** makes a folder an API endpoint (`GET`, `POST`, ...).
5. **`[id]`** in a folder name = a dynamic URL segment. `tasks/[id]/page.tsx` → `/tasks/abc123`.

Special files with automatic behavior: `loading.tsx` (loading UI), `error.tsx` (error UI), `not-found.tsx` (404 UI).

### Server Components vs Client Components

| | Server Component (default) | Client Component |
|---|---|---|
| Runs on | Server only | Browser (and server for first render) |
| Can use `useState`, `onClick` | ❌ | ✅ |
| Can read DB/files/secrets directly | ✅ | ❌ |
| How to opt in | Nothing (default) | First line: `"use client"` |

**Rule of thumb:** keep components on the server by default. Add `"use client"` only for the small parts that need interactivity.

### npm scripts

Open `package.json`. The generator created:

| Script | What it does |
|---|---|
| `npm run dev` | Dev server with hot reload |
| `npm run build` | Production build |
| `npm run start` | Run the production build |
| `npm run lint` | Run ESLint |

---

## Part 4: Git setup with test, stage, main

The generator already created a Git repo with `main`. Now create the `stage` and `test` branches. (If you did not use the generator's Git, run `git init` and commit first.)

💻 **Terminal**

```bash
git status                  # should be clean
git branch                  # shows: main

# Create the promotion branches from main
git branch stage
git branch test

# Start working from `test`
git switch test
git switch -c chore/setup-tooling
```

You are now on `chore/setup-tooling`, a short-lived branch. All work in this guide happens on short-lived branches created from `test`, then flows `test → stage → main`.

```text
feature branch ──▶ test ──▶ stage ──▶ main
```

---

## Part 5: Quality tooling (Prettier, Vitest, Husky, commitlint)

We set this up **before** writing app code so every commit is checked automatically.

### 5.1 Install the tools

💻 **Terminal**

```bash
# Formatter (+ Tailwind class sorting)
npm install -D prettier prettier-plugin-tailwindcss

# Unit testing
npm install -D vitest

# Git hooks + staged-file checks + commit message rules
npm install -D husky lint-staged @commitlint/cli @commitlint/config-conventional

# Runtime validation library (used later for env + API input)
npm install zod
```

### 5.2 Prettier config

📄 **File: `.prettierrc.json`** (project root) → **Action: create new file**

```json
{
  "semi": true,
  "singleQuote": false,
  "trailingComma": "all",
  "printWidth": 100,
  "plugins": ["prettier-plugin-tailwindcss"]
}
```

📄 **File: `.prettierignore`** (project root) → **Action: create new file**

```text
.next
node_modules
coverage
package-lock.json
public
```

### 5.3 Node version file

📄 **File: `.nvmrc`** (project root) → **Action: create new file**

```text
22
```

### 5.4 Update `package.json` scripts

📄 **File: `package.json`** → **Action: edit.** Find the `"scripts"` section and replace it with the block below. Do **not** touch `dependencies` or `devDependencies`. (If `npx husky init` later adds `"prepare": "husky"`, keep it.)

```json
"scripts": {
  "dev": "next dev",
  "build": "next build",
  "start": "next start",
  "lint": "eslint . --max-warnings=0",
  "lint:fix": "eslint . --fix",
  "format": "prettier --write .",
  "format:check": "prettier --check .",
  "typecheck": "tsc --noEmit",
  "test": "vitest run",
  "test:watch": "vitest"
},
```

Then add this block at the **top level** of `package.json` (a sibling of `"scripts"`, for example right after it). Mind the commas between sections:

```json
"lint-staged": {
  "*.{ts,tsx,js,jsx,mjs}": [
    "eslint --fix --max-warnings=0",
    "prettier --write"
  ],
  "*.{json,md,css,yml,yaml}": [
    "prettier --write"
  ]
},
```

### 5.4b Vitest config

📄 **File: `vitest.config.ts`** (project root) → **Action: create new file**

```ts
import path from "node:path";
import { defineConfig } from "vitest/config";

export default defineConfig({
  test: {
    environment: "node",
    include: ["src/**/*.test.ts"],
  },
  resolve: {
    alias: {
      // Lets tests use the same "@/..." imports as the app
      "@": path.resolve(__dirname, "src"),
    },
  },
});
```

### 5.5 commitlint config

📄 **File: `commitlint.config.mjs`** (project root) → **Action: create new file**

```js
export default {
  extends: ["@commitlint/config-conventional"],
  rules: {
    "header-max-length": [2, "always", 72],
  },
};
```

### 5.6 Husky hooks

💻 **Terminal**

```bash
npx husky init
```

This creates a `.husky/` folder with a sample `pre-commit` file and adds `"prepare": "husky"` to `package.json`. Now replace/create the three hooks:

📄 **File: `.husky/pre-commit`** → **Action: replace the whole content**

```sh
npx lint-staged
npm run typecheck
```

📄 **File: `.husky/commit-msg`** → **Action: create new file**

```sh
npx --no -- commitlint --edit "$1"
```

📄 **File: `.husky/pre-push`** → **Action: create new file**

```sh
npm run lint
npm run typecheck
npm run test
npm run build
```

> **macOS/Linux only:** make sure the hook files are executable:
> `chmod +x .husky/pre-commit .husky/commit-msg .husky/pre-push`

### 5.7 Format the generated code once, then commit

💻 **Terminal**

```bash
npm run format
npm run lint
npm run typecheck
```

(`npm run test` will say "no test files found" for now. That is expected until Part 6.)

Now make your **first conventional commit**:

```bash
git add .
git commit -m "chore: add prettier, vitest, husky and commitlint"
```

You should see lint-staged and the type-check run. 🎉 Now prove the commit message rule works:

```bash
git commit --allow-empty -m "did some stuff"
```

❌ This must **fail** with a commitlint error. Valid examples look like:

```text
feat(tasks): add task list page
fix(api): return 400 for empty title
refactor: extract task store
chore: update dependencies
```

Merge this work into `test` and start the feature branch:

```bash
git switch test
git merge --no-ff chore/setup-tooling -m "chore: merge tooling setup into test"
git branch -d chore/setup-tooling
git switch -c feat/tasks-app
```

> On a team with GitLab you would push the branch and open a **Merge Request** instead of merging locally. See [Part 11](#part-11-push-and-promote-through-branches).

---

## Part 6: Build the app

You are on branch `feat/tasks-app`. Keep `npm run dev` running in one terminal. Pages reload automatically when you save.

### Step 1: Create the folders

💻 **Terminal**

macOS / Linux / Git Bash:

```bash
mkdir -p src/types src/lib src/components
mkdir -p src/app/api/health src/app/api/tasks/\[id\] src/app/tasks/\[id\]
```

Windows PowerShell:

```powershell
mkdir src\types, src\lib, src\components, src\app\api\health, src\app\api\tasks, src\app\tasks
```

> The folders named `[id]` (square brackets are part of the real name!) are easiest to create in VS Code: right-click `src/app/api/tasks` → *New Folder* → type `[id]`. Do the same under `src/app/tasks`.

### Step 2: The Task type

📄 **File: `src/types/task.ts`** → **Action: create new file**

```ts
export type Task = {
  id: string;
  title: string;
  completed: boolean;
  createdAt: string; // ISO date string
};
```

### Step 3: Environment validation

This makes the app **fail fast** if configuration is wrong, instead of breaking mysteriously later.

📄 **File: `src/lib/env.ts`** → **Action: create new file**

```ts
import { z } from "zod";

const schema = z.object({
  NODE_ENV: z.enum(["development", "test", "production"]).default("development"),
  LOG_LEVEL: z.enum(["debug", "info", "warn", "error"]).default("info"),
  APP_VERSION: z.string().default("dev"),
});

// Server-side configuration. Never import this file in a Client Component.
export const env = schema.parse({
  NODE_ENV: process.env.NODE_ENV,
  LOG_LEVEL: process.env.LOG_LEVEL,
  APP_VERSION: process.env.APP_VERSION,
});
```

### Step 4: The in-memory task store

This acts as our "database" for learning. It lives in server memory, so data resets when the server restarts. (You will swap it for a real database later.)

📄 **File: `src/lib/tasks-store.ts`** → **Action: create new file**

```ts
import { randomUUID } from "node:crypto";
import type { Task } from "@/types/task";

// In development, Next.js reloads modules often. Keeping the array on `globalThis`
// stops the data from being wiped on every hot reload.
const globalForTasks = globalThis as unknown as { __tasks?: Task[] };

if (!globalForTasks.__tasks) {
  globalForTasks.__tasks = [
    {
      id: randomUUID(),
      title: "Read the Next.js App Router guide",
      completed: false,
      createdAt: new Date().toISOString(),
    },
    {
      id: randomUUID(),
      title: "Set up Git hooks",
      completed: true,
      createdAt: new Date().toISOString(),
    },
  ];
}

const tasks: Task[] = globalForTasks.__tasks;

export function listTasks(): Task[] {
  return [...tasks];
}

export function getTask(id: string): Task | undefined {
  return tasks.find((task) => task.id === id);
}

export function createTask(title: string): Task {
  const task: Task = {
    id: randomUUID(),
    title,
    completed: false,
    createdAt: new Date().toISOString(),
  };
  tasks.unshift(task); // newest first
  return task;
}

export function updateTask(id: string, patch: { completed: boolean }): Task | undefined {
  const task = getTask(id);
  if (!task) return undefined;
  task.completed = patch.completed;
  return task;
}

export function deleteTask(id: string): boolean {
  const index = tasks.findIndex((task) => task.id === id);
  if (index === -1) return false;
  tasks.splice(index, 1);
  return true;
}

// Used by tests to start from a clean slate
export function resetTasks(): void {
  tasks.length = 0;
}
```

### Step 5: A unit test for the store

📄 **File: `src/lib/tasks-store.test.ts`** → **Action: create new file**

```ts
import { beforeEach, describe, expect, it } from "vitest";
import {
  createTask,
  deleteTask,
  getTask,
  listTasks,
  resetTasks,
  updateTask,
} from "@/lib/tasks-store";

describe("tasks-store", () => {
  beforeEach(() => {
    resetTasks();
  });

  it("creates a task that is not completed", () => {
    const task = createTask("Write tests");
    expect(task.title).toBe("Write tests");
    expect(task.completed).toBe(false);
    expect(task.id).toBeTruthy();
  });

  it("lists the newest task first", () => {
    createTask("first");
    createTask("second");
    const titles = listTasks().map((t) => t.title);
    expect(titles).toEqual(["second", "first"]);
  });

  it("finds a task by id", () => {
    const created = createTask("find me");
    expect(getTask(created.id)?.title).toBe("find me");
    expect(getTask("does-not-exist")).toBeUndefined();
  });

  it("marks a task as completed", () => {
    const created = createTask("finish me");
    const updated = updateTask(created.id, { completed: true });
    expect(updated?.completed).toBe(true);
  });

  it("returns undefined when updating a missing task", () => {
    expect(updateTask("nope", { completed: true })).toBeUndefined();
  });

  it("deletes a task", () => {
    const created = createTask("delete me");
    expect(deleteTask(created.id)).toBe(true);
    expect(listTasks()).toHaveLength(0);
    expect(deleteTask(created.id)).toBe(false);
  });
});
```

💻 **Terminal**: run the tests:

```bash
npm run test
```

You should see **6 passed**. ✅

### Step 6: API route: health check

Monitoring tools and Docker use this to know the app is alive.

📄 **File: `src/app/api/health/route.ts`** → **Action: create new file**

```ts
import { NextResponse } from "next/server";
import { env } from "@/lib/env";

// Always run on each request, never cache or pre-render
export const dynamic = "force-dynamic";

export function GET() {
  return NextResponse.json({
    status: "ok",
    version: env.APP_VERSION,
    uptimeSeconds: Math.round(process.uptime()),
    timestamp: new Date().toISOString(),
  });
}
```

### Step 7: API route: list and create tasks

📄 **File: `src/app/api/tasks/route.ts`** → **Action: create new file**

```ts
import { NextResponse } from "next/server";
import { z } from "zod";
import { createTask, listTasks } from "@/lib/tasks-store";

export const dynamic = "force-dynamic";

const createSchema = z.object({
  title: z.string().trim().min(1).max(100),
});

// GET /api/tasks → list all tasks
export function GET() {
  return NextResponse.json(listTasks());
}

// POST /api/tasks → create a task. Body: { "title": "Buy milk" }
export async function POST(request: Request) {
  let body: unknown;
  try {
    body = await request.json();
  } catch {
    return NextResponse.json({ error: "Body must be valid JSON" }, { status: 400 });
  }

  const parsed = createSchema.safeParse(body);
  if (!parsed.success) {
    return NextResponse.json({ error: "Title is required (1 to 100 characters)" }, { status: 400 });
  }

  const task = createTask(parsed.data.title);
  return NextResponse.json(task, { status: 201 });
}
```

### Step 8: API route: update and delete one task

📄 **File: `src/app/api/tasks/[id]/route.ts`** → **Action: create new file**

```ts
import { NextResponse } from "next/server";
import { z } from "zod";
import { deleteTask, updateTask } from "@/lib/tasks-store";

export const dynamic = "force-dynamic";

// In Next.js 15+ route params are a Promise, so we must `await` them.
type RouteContext = { params: Promise<{ id: string }> };

const patchSchema = z.object({
  completed: z.boolean(),
});

// PATCH /api/tasks/:id → body: { "completed": true }
export async function PATCH(request: Request, { params }: RouteContext) {
  const { id } = await params;

  let body: unknown;
  try {
    body = await request.json();
  } catch {
    return NextResponse.json({ error: "Body must be valid JSON" }, { status: 400 });
  }

  const parsed = patchSchema.safeParse(body);
  if (!parsed.success) {
    return NextResponse.json({ error: "completed must be true or false" }, { status: 400 });
  }

  const task = updateTask(id, parsed.data);
  if (!task) {
    return NextResponse.json({ error: "Task not found" }, { status: 404 });
  }
  return NextResponse.json(task);
}

// DELETE /api/tasks/:id
export async function DELETE(_request: Request, { params }: RouteContext) {
  const { id } = await params;
  const removed = deleteTask(id);
  if (!removed) {
    return NextResponse.json({ error: "Task not found" }, { status: 404 });
  }
  return new NextResponse(null, { status: 204 });
}
```

### Step 9: Global styles

📄 **File: `src/app/globals.css`** → **Action: replace the whole content**

```css
@import "tailwindcss";

body {
  @apply bg-slate-50 text-slate-900 antialiased;
}
```

### Step 10: Navbar (Client Component)

It needs `usePathname` to highlight the active link, so it must be a Client Component.

📄 **File: `src/components/Navbar.tsx`** → **Action: create new file**

```tsx
"use client";

import Link from "next/link";
import { usePathname } from "next/navigation";

const links = [
  { href: "/", label: "Home" },
  { href: "/tasks", label: "Tasks" },
];

export default function Navbar() {
  const pathname = usePathname();
  // NEXT_PUBLIC_ variables are readable in the browser (they are baked in at build time)
  const appName = process.env.NEXT_PUBLIC_APP_NAME ?? "TaskFlow";

  return (
    <header className="border-b border-slate-200 bg-white">
      <nav className="mx-auto flex max-w-3xl items-center justify-between px-4 py-3">
        <Link href="/" className="text-lg font-bold text-indigo-600">
          {appName}
        </Link>
        <ul className="flex gap-4">
          {links.map((link) => {
            const active = pathname === link.href;
            return (
              <li key={link.href}>
                <Link
                  href={link.href}
                  className={
                    active
                      ? "font-semibold text-indigo-600"
                      : "text-slate-600 hover:text-slate-900"
                  }
                >
                  {link.label}
                </Link>
              </li>
            );
          })}
        </ul>
      </nav>
    </header>
  );
}
```

### Step 11: Root layout

📄 **File: `src/app/layout.tsx`** → **Action: replace the whole content**

```tsx
import type { Metadata } from "next";
import type { ReactNode } from "react";
import { Inter } from "next/font/google";
import "./globals.css";
import Navbar from "@/components/Navbar";

const inter = Inter({ subsets: ["latin"] });

export const metadata: Metadata = {
  title: {
    default: "TaskFlow",
    template: "%s | TaskFlow",
  },
  description: "A small task manager built with Next.js",
};

export default function RootLayout({ children }: Readonly<{ children: ReactNode }>) {
  return (
    <html lang="en">
      <body className={inter.className}>
        <Navbar />
        <main className="mx-auto max-w-3xl px-4 py-8">{children}</main>
      </body>
    </html>
  );
}
```

### Step 12: Home page

📄 **File: `src/app/page.tsx`** → **Action: replace the whole content**

```tsx
import Link from "next/link";

export default function HomePage() {
  return (
    <section className="space-y-6">
      <h1 className="text-3xl font-bold">Welcome to TaskFlow 👋</h1>
      <p className="text-slate-600">
        A tiny task manager built to learn Next.js, from first commit to production.
      </p>
      <div className="flex gap-3">
        <Link
          href="/tasks"
          className="rounded-lg bg-indigo-600 px-4 py-2 font-medium text-white hover:bg-indigo-700"
        >
          Open my tasks
        </Link>
        <a
          href="/api/health"
          className="rounded-lg border border-slate-300 px-4 py-2 font-medium hover:bg-slate-100"
        >
          API health
        </a>
      </div>
    </section>
  );
}
```

### Step 13: Interactive tasks UI (Client Component)

📄 **File: `src/components/TasksClient.tsx`** → **Action: create new file**

```tsx
"use client";

import Link from "next/link";
import { useState } from "react";
import type { FormEvent } from "react";
import type { Task } from "@/types/task";

type Props = {
  initialTasks: Task[];
};

export default function TasksClient({ initialTasks }: Props) {
  const [tasks, setTasks] = useState<Task[]>(initialTasks);
  const [title, setTitle] = useState("");
  const [error, setError] = useState<string | null>(null);
  const [loading, setLoading] = useState(false);

  async function handleAdd(event: FormEvent<HTMLFormElement>) {
    event.preventDefault();
    setError(null);

    const trimmed = title.trim();
    if (!trimmed) {
      setError("Please type a task title.");
      return;
    }

    setLoading(true);
    try {
      const res = await fetch("/api/tasks", {
        method: "POST",
        headers: { "Content-Type": "application/json" },
        body: JSON.stringify({ title: trimmed }),
      });
      if (!res.ok) {
        const data = await res.json();
        throw new Error(data.error ?? "Could not add the task.");
      }
      const created: Task = await res.json();
      setTasks((prev) => [created, ...prev]);
      setTitle("");
    } catch (err) {
      setError(err instanceof Error ? err.message : "Something went wrong.");
    } finally {
      setLoading(false);
    }
  }

  async function handleToggle(task: Task) {
    setError(null);
    const res = await fetch(`/api/tasks/${task.id}`, {
      method: "PATCH",
      headers: { "Content-Type": "application/json" },
      body: JSON.stringify({ completed: !task.completed }),
    });
    if (!res.ok) {
      setError("Could not update the task.");
      return;
    }
    const updated: Task = await res.json();
    setTasks((prev) => prev.map((t) => (t.id === updated.id ? updated : t)));
  }

  async function handleDelete(id: string) {
    setError(null);
    const res = await fetch(`/api/tasks/${id}`, { method: "DELETE" });
    if (!res.ok) {
      setError("Could not delete the task.");
      return;
    }
    setTasks((prev) => prev.filter((t) => t.id !== id));
  }

  return (
    <div className="space-y-4">
      <form onSubmit={handleAdd} className="flex gap-2">
        <input
          value={title}
          onChange={(e) => setTitle(e.target.value)}
          placeholder="What needs to be done?"
          maxLength={100}
          className="flex-1 rounded-lg border border-slate-300 bg-white px-3 py-2 outline-none focus:border-indigo-500"
        />
        <button
          type="submit"
          disabled={loading}
          className="rounded-lg bg-indigo-600 px-4 py-2 font-medium text-white hover:bg-indigo-700 disabled:opacity-50"
        >
          {loading ? "Adding..." : "Add"}
        </button>
      </form>

      {error && (
        <p role="alert" className="rounded-lg bg-red-50 px-3 py-2 text-sm text-red-700">
          {error}
        </p>
      )}

      {tasks.length === 0 ? (
        <p className="text-slate-500">No tasks yet. Add your first one above.</p>
      ) : (
        <ul className="space-y-2">
          {tasks.map((task) => (
            <li
              key={task.id}
              className="flex items-center gap-3 rounded-lg border border-slate-200 bg-white px-3 py-2"
            >
              <input
                type="checkbox"
                checked={task.completed}
                onChange={() => handleToggle(task)}
                aria-label={`Mark "${task.title}" as ${task.completed ? "not done" : "done"}`}
                className="h-4 w-4"
              />
              <Link
                href={`/tasks/${task.id}`}
                className={`flex-1 hover:underline ${
                  task.completed ? "text-slate-400 line-through" : ""
                }`}
              >
                {task.title}
              </Link>
              <button
                onClick={() => handleDelete(task.id)}
                className="text-sm text-red-600 hover:underline"
              >
                Delete
              </button>
            </li>
          ))}
        </ul>
      )}
    </div>
  );
}
```

### Step 14: Tasks page (Server Component)

This page runs on the server, reads the store directly, and hands the data to the client component.

📄 **File: `src/app/tasks/page.tsx`** → **Action: create new file**

```tsx
import type { Metadata } from "next";
import TasksClient from "@/components/TasksClient";
import { listTasks } from "@/lib/tasks-store";

export const metadata: Metadata = {
  title: "Tasks",
};

// Our data changes at runtime, so render on every request instead of at build time
export const dynamic = "force-dynamic";

export default function TasksPage() {
  const tasks = listTasks(); // runs on the server

  return (
    <section className="space-y-4">
      <h1 className="text-2xl font-bold">My tasks</h1>
      <TasksClient initialTasks={tasks} />
    </section>
  );
}
```

### Step 15: Task detail page (dynamic route)

📄 **File: `src/app/tasks/[id]/page.tsx`** → **Action: create new file**

```tsx
import type { Metadata } from "next";
import Link from "next/link";
import { notFound } from "next/navigation";
import { getTask } from "@/lib/tasks-store";

export const dynamic = "force-dynamic";

// In Next.js 15+ `params` is a Promise
type Props = { params: Promise<{ id: string }> };

export async function generateMetadata({ params }: Props): Promise<Metadata> {
  const { id } = await params;
  const task = getTask(id);
  return { title: task ? task.title : "Task not found" };
}

export default async function TaskDetailPage({ params }: Props) {
  const { id } = await params;
  const task = getTask(id);

  if (!task) {
    notFound(); // shows src/app/not-found.tsx
  }

  return (
    <article className="space-y-4">
      <Link href="/tasks" className="text-sm text-indigo-600 hover:underline">
        ← Back to tasks
      </Link>
      <h1 className="text-2xl font-bold">{task.title}</h1>
      <dl className="space-y-1 text-slate-600">
        <div>
          <dt className="inline font-medium">Status: </dt>
          <dd className="inline">{task.completed ? "Done ✅" : "In progress ⏳"}</dd>
        </div>
        <div>
          <dt className="inline font-medium">Created: </dt>
          <dd className="inline">{new Date(task.createdAt).toUTCString()}</dd>
        </div>
        <div>
          <dt className="inline font-medium">ID: </dt>
          <dd className="inline font-mono text-sm">{task.id}</dd>
        </div>
      </dl>
    </article>
  );
}
```

### Step 16: Loading, error, and 404 pages

📄 **File: `src/app/tasks/loading.tsx`** → **Action: create new file**

```tsx
export default function Loading() {
  return <p className="text-slate-500">Loading tasks...</p>;
}
```

📄 **File: `src/app/error.tsx`** → **Action: create new file**

(Error boundaries must be Client Components.)

```tsx
"use client";

import { useEffect } from "react";

type Props = {
  error: Error & { digest?: string };
  reset: () => void;
};

export default function GlobalError({ error, reset }: Props) {
  useEffect(() => {
    // In production, send this to an error tracker such as Sentry
    console.error(error);
  }, [error]);

  return (
    <section className="space-y-4">
      <h1 className="text-2xl font-bold">Something went wrong</h1>
      <p className="text-slate-600">An unexpected error occurred. Please try again.</p>
      <button
        onClick={reset}
        className="rounded-lg bg-indigo-600 px-4 py-2 font-medium text-white hover:bg-indigo-700"
      >
        Try again
      </button>
    </section>
  );
}
```

📄 **File: `src/app/not-found.tsx`** → **Action: create new file**

```tsx
import Link from "next/link";

export default function NotFound() {
  return (
    <section className="space-y-4">
      <h1 className="text-2xl font-bold">404: Page not found</h1>
      <p className="text-slate-600">We could not find what you were looking for.</p>
      <Link href="/" className="text-indigo-600 hover:underline">
        Go back home
      </Link>
    </section>
  );
}
```

### Step 17: Next.js config (security headers + Docker support)

📄 **File: `next.config.ts`** → **Action: replace the whole content**

```ts
import type { NextConfig } from "next";

const nextConfig: NextConfig = {
  // Produces a small self-contained server in .next/standalone (used by Docker later)
  output: "standalone",

  // Do not advertise the framework in response headers
  poweredByHeader: false,

  async headers() {
    return [
      {
        source: "/:path*",
        headers: [
          { key: "X-Content-Type-Options", value: "nosniff" },
          { key: "X-Frame-Options", value: "DENY" },
          { key: "Referrer-Policy", value: "strict-origin-when-cross-origin" },
        ],
      },
    ];
  },
};

export default nextConfig;
```

### Step 18: Format, check, and commit

💻 **Terminal**

```bash
npm run format
npm run lint
npm run typecheck
npm run test
npm run build
```

Everything should pass. Then commit (the hooks run automatically):

```bash
git add .
git commit -m "feat(tasks): add task manager pages, API routes and tests"
```

---

## Part 7: Run and test everything

### 7.1 Click through the UI

With `npm run dev` running, open:

| URL | You should see |
|---|---|
| http://localhost:3000 | Home page with the navbar |
| http://localhost:3000/tasks | Two sample tasks; add, check, and delete tasks |
| http://localhost:3000/tasks/ | (click a task title) the detail page |
| http://localhost:3000/tasks/does-not-exist | The custom 404 page |
| http://localhost:3000/api/health | JSON `{ "status": "ok", ... }` |

### 7.2 Test the API from the terminal

💻 **Terminal** (macOS/Linux/Git Bash; on Windows PowerShell use `curl.exe` instead of `curl`)

```bash
# List tasks
curl http://localhost:3000/api/tasks

# Create a task
curl -X POST http://localhost:3000/api/tasks \
  -H "Content-Type: application/json" \
  -d '{"title":"Learn Docker"}'

# Validation error (empty title) → expect HTTP 400
curl -i -X POST http://localhost:3000/api/tasks \
  -H "Content-Type: application/json" \
  -d '{"title":"   "}'

# Mark done (replace TASK_ID with an id from the list)
curl -X PATCH http://localhost:3000/api/tasks/TASK_ID \
  -H "Content-Type: application/json" \
  -d '{"completed":true}'

# Delete
curl -i -X DELETE http://localhost:3000/api/tasks/TASK_ID
```

### 7.3 Test the production build

```bash
npm run build
npm run start
```

Visit http://localhost:3000. This is how the app behaves in production (faster, no hot reload). Stop it with `Ctrl + C`.

> If `npm run start` warns about `output: "standalone"`, that is fine for local testing. Docker uses the standalone server in Part 9.

---

## Part 8: Environment variables

### 8.1 How Next.js loads them

| File | Purpose | Commit to Git? |
|---|---|---|
| `.env.local` | Your personal local values and secrets | **Never** |
| `.env.example` | Template listing the variable names with fake values | **Yes** |
| `.env.production` | Production defaults (non-secret only) | Only if no secrets |

- Variables are available on the **server** as `process.env.NAME`.
- Only variables starting with **`NEXT_PUBLIC_`** are sent to the **browser** (and are baked in at **build time**). **Never put secrets in a `NEXT_PUBLIC_` variable.**
- Restart `npm run dev` after changing env files.

### 8.2 Create the files

📄 **File: `.env.example`** → **Action: create new file**

```bash
# Copy this file to .env.local and fill in real values.
# Public (visible in the browser)
NEXT_PUBLIC_APP_NAME=TaskFlow

# Server only
LOG_LEVEL=info
APP_VERSION=dev
```

📄 **File: `.env.local`** → **Action: create new file**

```bash
NEXT_PUBLIC_APP_NAME=TaskFlow (local)
LOG_LEVEL=debug
APP_VERSION=local
```

Restart the dev server (`Ctrl + C`, then `npm run dev`). The navbar now shows **TaskFlow (local)**, and `/api/health` reports `"version": "local"`.

### 8.3 Make sure `.env.example` is committed

The generated `.gitignore` ignores every `.env*` file, including `.env.example`. Allow the example file:

📄 **File: `.gitignore`** → **Action: edit.** Find the line `.env*` and add one line right below it:

```text
.env*
!.env.example
```

💻 **Terminal**

```bash
git status                # .env.example should appear; .env.local must NOT
git add .
git commit -m "chore: add environment variable template"
```

---

## Part 9: Docker

Docker packages your app and everything it needs into an **image** that runs the same way on any machine.

### 9.1 Install Docker

Install **Docker Desktop** (Windows/macOS) or Docker Engine (Linux) from https://docs.docker.com/get-docker/ and verify:

💻 **Terminal**

```bash
docker --version
docker compose version
```

### 9.2 Dockerfile

📄 **File: `Dockerfile`** (project root, no extension) → **Action: create new file**

```dockerfile
# syntax=docker/dockerfile:1

# ---------- Stage 1: install dependencies ----------
FROM node:22-alpine AS deps
WORKDIR /app
COPY package.json package-lock.json ./
# --ignore-scripts: skip the "prepare" (husky) script, there is no .git inside the image
RUN npm ci --ignore-scripts

# ---------- Stage 2: build the app ----------
FROM node:22-alpine AS build
WORKDIR /app
COPY --from=deps /app/node_modules ./node_modules
COPY . .

# NEXT_PUBLIC_ variables are baked in at BUILD time, so pass them as build args
ARG NEXT_PUBLIC_APP_NAME=TaskFlow
ENV NEXT_PUBLIC_APP_NAME=$NEXT_PUBLIC_APP_NAME
ENV NEXT_TELEMETRY_DISABLED=1

RUN npm run build

# ---------- Stage 3: tiny runtime image ----------
FROM node:22-alpine AS runner
WORKDIR /app

ENV NODE_ENV=production
ENV NEXT_TELEMETRY_DISABLED=1
ENV PORT=3000
ENV HOSTNAME=0.0.0.0

# Run as a non-root user (security best practice)
RUN addgroup -S nextjs && adduser -S nextjs -G nextjs

COPY --from=build --chown=nextjs:nextjs /app/public ./public
COPY --from=build --chown=nextjs:nextjs /app/.next/standalone ./
COPY --from=build --chown=nextjs:nextjs /app/.next/static ./.next/static

USER nextjs
EXPOSE 3000

HEALTHCHECK --interval=30s --timeout=3s --start-period=15s --retries=3 \
  CMD wget -qO- http://127.0.0.1:3000/api/health || exit 1

CMD ["node", "server.js"]
```

> Your project must have a `public/` folder for the `COPY` line to work. The generator creates one. If you deleted it, run `mkdir public`.

### 9.3 `.dockerignore`

Keeps the build fast and stops secrets leaking into the image.

📄 **File: `.dockerignore`** → **Action: create new file**

```text
node_modules
.next
.git
.husky
.env
.env.*
coverage
npm-debug.log
Dockerfile
docker-compose.yml
.vscode
*.md
```

### 9.4 Docker Compose

📄 **File: `docker-compose.yml`** → **Action: create new file**

```yaml
services:
  web:
    build:
      context: .
      args:
        NEXT_PUBLIC_APP_NAME: "TaskFlow"
    image: taskflow:local
    ports:
      - "3000:3000"
    environment:
      LOG_LEVEL: info
      APP_VERSION: docker-local
    restart: unless-stopped
```

### 9.5 Build and run

💻 **Terminal** (stop `npm run dev` first if it is using port 3000)

```bash
# Build the image and start the container in the background
docker compose up --build -d

# See it running (STATUS should become "healthy" after ~15 seconds)
docker ps

# Follow the logs
docker compose logs -f web

# Test
curl http://localhost:3000/api/health
```

Open http://localhost:3000. Your app is now running **inside a container**. 🐳

Useful commands:

```bash
docker compose ps                 # status
docker compose restart web        # restart
docker compose down               # stop and remove
docker image ls taskflow          # check the image size (should be well under 300 MB)
docker exec -it $(docker ps -qf name=web) whoami   # prints "nextjs" (not root)
```

### 9.6 Commit

```bash
git add .
git commit -m "build(docker): add multi-stage Dockerfile and compose file"
```

---

## Part 10: CI pipeline

CI re-runs your checks on the server on every push, because local hooks can be bypassed with `--no-verify` but CI cannot.

📄 **File: `.gitlab-ci.yml`** (project root) → **Action: create new file**

```yaml
stages:
  - quality
  - test
  - build

workflow:
  rules:
    - if: $CI_PIPELINE_SOURCE == "merge_request_event"
    - if: $CI_COMMIT_BRANCH =~ /^(test|stage|main)$/

default:
  image: node:22-alpine
  cache:
    key:
      files:
        - package-lock.json
    paths:
      - .npm/

variables:
  NPM_CONFIG_CACHE: "$CI_PROJECT_DIR/.npm"
  HUSKY: "0"   # do not install git hooks inside CI

.install: &install
  before_script:
    - npm ci --prefer-offline

lint:
  stage: quality
  <<: *install
  script:
    - npm run lint

format:
  stage: quality
  <<: *install
  script:
    - npm run format:check

typecheck:
  stage: quality
  <<: *install
  script:
    - npm run typecheck

unit-tests:
  stage: test
  <<: *install
  script:
    - npm run test

build:
  stage: build
  <<: *install
  script:
    - npm run build
```

> **Using GitHub instead of GitLab?** Create `.github/workflows/ci.yml`:
>
> ```yaml
> name: CI
> on:
>   pull_request:
>     branches: [test, stage, main]
>   push:
>     branches: [test, stage, main]
> jobs:
>   checks:
>     runs-on: ubuntu-latest
>     env:
>       HUSKY: "0"
>     steps:
>       - uses: actions/checkout@v4
>       - uses: actions/setup-node@v4
>         with:
>           node-version: 22
>           cache: npm
>       - run: npm ci
>       - run: npm run lint
>       - run: npm run format:check
>       - run: npm run typecheck
>       - run: npm run test
>       - run: npm run build
> ```

Commit it:

💻 **Terminal**

```bash
git add .
git commit -m "ci: add quality, test and build pipeline"
```

---

## Part 11: Push and promote through branches

### 11.1 Create a remote repository

On GitLab (or GitHub) create a new **empty** project named `taskflow` (no README). Copy its URL, then:

💻 **Terminal**

```bash
git remote add origin https://gitlab.com/YOUR-USERNAME/taskflow.git
git remote -v
```

### 11.2 Push the long-lived branches

```bash
git push -u origin main
git push -u origin stage
git push -u origin test
```

### 11.3 Push your feature branch (pre-push hook runs here)

```bash
git push -u origin feat/tasks-app
```

The `pre-push` hook runs lint, type-check, tests, and the production build. It takes a minute. If any fail, the push is blocked. 🛡️

### 11.4 Open a Merge Request

In GitLab: **Merge requests → New merge request**: source `feat/tasks-app`, target **`test`**. Wait for the pipeline to go green, get a review, then **Squash and merge**.

### 11.5 Promote

Repeat the same process with Merge Requests:

```text
test  ──MR──▶  stage  ──MR──▶  main
```

### 11.6 Protect the branches

In GitLab: **Settings → Repository → Protected branches**:

| Branch | Allowed to push | Allowed to merge |
|---|---|---|
| `main` | No one | Maintainers |
| `stage` | No one | Maintainers |
| `test` | No one | Developers + Maintainers |

And **Settings → Merge requests**: enable *Pipelines must succeed* and *All threads must be resolved*.

### 11.7 Daily workflow from now on

```bash
git switch test
git pull origin test
git switch -c feat/my-next-feature
# ...code...
git add .
git commit -m "feat(scope): describe the change"
git push -u origin feat/my-next-feature
# open a Merge Request into `test`
```

---

## Troubleshooting

| Problem | Fix |
|---|---|
| `node: command not found` or old Node version | Install Node 20.9+ (22 recommended) and reopen the terminal |
| `Port 3000 is already in use` | Stop the other process, or run `npm run dev -- -p 3001` |
| Changes not showing | Hard-refresh the browser (`Ctrl+Shift+R`); restart `npm run dev` |
| Commit rejected: `subject may not be empty` / `type may not be empty` | Use Conventional Commits: `feat: message`, `fix: message`, ... |
| Commit rejected by ESLint | Run `npm run lint:fix`, fix what remains, commit again |
| Commit rejected by type check | Fix the TypeScript errors printed by `npm run typecheck` |
| Hooks do not run at all | Run `npm install` again (it runs `husky`), and check `.husky/` files exist; on macOS/Linux run `chmod +x .husky/*` |
| `Module not found: Can't resolve '@/...'` | Check `tsconfig.json` has `"paths": { "@/*": ["./src/*"] }` (the generator adds it) |
| Tests fail with `Cannot find module '@/lib/...'` | Check `vitest.config.ts` has the `resolve.alias` block |
| `next build` fails fetching Google Fonts | `next/font/google` needs internet during build; check your connection/proxy |
| Docker build fails at `COPY /app/public` | Create an empty `public` folder: `mkdir public` |
| Docker container is `unhealthy` | Run `docker compose logs web`; check `curl localhost:3000/api/health` |
| Data disappears after restart | Expected: the store is in memory. Add a real database (see Next steps) |
| Hydration error in the browser | Do not render values that differ between server and browser (like `Date.now()`) directly in components |
| `Pages` or `params` type errors | In Next.js 15+ `params` is a **Promise**. Use `const { id } = await params;` |

---

## Final folder structure

```text
taskflow/
├── .husky/
│   ├── commit-msg
│   ├── pre-commit
│   └── pre-push
├── public/
├── src/
│   ├── app/
│   │   ├── api/
│   │   │   ├── health/
│   │   │   │   └── route.ts
│   │   │   └── tasks/
│   │   │       ├── [id]/
│   │   │       │   └── route.ts
│   │   │       └── route.ts
│   │   ├── tasks/
│   │   │   ├── [id]/
│   │   │   │   └── page.tsx
│   │   │   ├── loading.tsx
│   │   │   └── page.tsx
│   │   ├── error.tsx
│   │   ├── favicon.ico
│   │   ├── globals.css
│   │   ├── layout.tsx
│   │   ├── not-found.tsx
│   │   └── page.tsx
│   ├── components/
│   │   ├── Navbar.tsx
│   │   └── TasksClient.tsx
│   ├── lib/
│   │   ├── env.ts
│   │   ├── tasks-store.test.ts
│   │   └── tasks-store.ts
│   └── types/
│       └── task.ts
├── .dockerignore
├── .env.example
├── .env.local                 (not committed)
├── .gitignore
├── .gitlab-ci.yml
├── .nvmrc
├── .prettierignore
├── .prettierrc.json
├── commitlint.config.mjs
├── docker-compose.yml
├── Dockerfile
├── eslint.config.mjs
├── next.config.ts
├── package.json
├── postcss.config.mjs
├── tsconfig.json
└── vitest.config.ts
```

### Where each file fits (request flow)

```text
Browser ──▶ /tasks ──▶ layout.tsx (Navbar) ──▶ tasks/page.tsx (server: reads store)
                                                    │
                                                    ▼
                                           TasksClient.tsx (browser)
                                                    │ fetch()
                                                    ▼
                                      /api/tasks, /api/tasks/[id]  (route.ts)
                                                    │
                                                    ▼
                                           lib/tasks-store.ts
```

---

## Next steps

You now have a project with a professional workflow. Suggested upgrades, in order:

1. **Add a real database.** Postgres + [Prisma](https://www.prisma.io) or [Drizzle](https://orm.drizzle.team). Replace the functions in `tasks-store.ts` with database queries. Your tests show what behavior to keep.
2. **Add authentication.** [Auth.js](https://authjs.dev) or [Clerk](https://clerk.com). Protect `/tasks` and attach tasks to users.
3. **Use Server Actions** for form submissions instead of `fetch` to your own API.
4. **Add end-to-end tests** with [Playwright](https://playwright.dev): open `/tasks`, add a task, assert it appears. Run them in CI.
5. **Add structured logging and error tracking** (`pino`, Sentry).
6. **Deploy:** push the image from CI to a container registry, then deploy to a VPS with Docker Compose + Caddy (see the DevOps guide), or to a platform like Vercel.
7. **Add a deploy stage** to `.gitlab-ci.yml` for `stage` (automatic) and `main` (manual approval), with a health-check-based rollback.

### Quick command reference

```bash
npm run dev            # develop
npm run lint           # ESLint
npm run format         # Prettier
npm run typecheck      # TypeScript
npm run test           # Vitest
npm run build          # production build
docker compose up --build -d     # run in Docker
docker compose down              # stop Docker
```

Happy building! 🚀

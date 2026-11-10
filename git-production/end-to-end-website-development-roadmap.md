# End-to-End Website Development Roadmap

**Stack:** Next.js (App Router) · React · TypeScript · Payload CMS · Node.js · Express.js (only when needed)
**Use it for:** Building a production-ready website from the first client call to long-term maintenance. Copy this file into each new project and tick the checklists as you go.

---

## How to Use This Roadmap

- Work through the phases **in order**. Each phase ends with a checklist; do not move on until it is complete.
- Items marked **(Optional)** explain when you actually need them.
- Items marked **[Automate]** should run in CI or a script. Items marked **[Manual]** need a human to check.
- Phases 8–13 (SEO, accessibility, performance, security, testing, analytics) are not "end of project" tasks. Plan them in Phase 1, build them in Phases 4–7, and verify them in their own phase.

### Table of Contents

0. [Key Concepts: Environments, Hosting, Domains, DNS, Deployment](#phase-0-key-concepts)
1. [Project Planning](#phase-1-project-planning)
2. [UI/UX Design](#phase-2-uiux-design)
3. [Project Setup](#phase-3-project-setup)
4. [Frontend Development](#phase-4-frontend-development)
5. [Backend Development](#phase-5-backend-development)
6. [CMS Integration (Payload)](#phase-6-cms-integration-payload-cms)
7. [Database Integration](#phase-7-database-integration)
8. [SEO Implementation](#phase-8-seo-implementation)
9. [Accessibility](#phase-9-accessibility)
10. [Performance Optimization](#phase-10-performance-optimization)
11. [Security](#phase-11-security)
12. [Testing and Quality Assurance](#phase-12-testing-and-quality-assurance)
13. [Analytics and Monitoring](#phase-13-analytics-and-monitoring)
14. [CI/CD and DevOps](#phase-14-cicd-and-devops)
15. [Deployment and Hosting](#phase-15-deployment-and-hosting)
16. [Domain and DNS Configuration](#phase-16-domain-and-dns-configuration)
17. [Production Launch Checklist](#phase-17-production-launch-checklist)
18. [Post-Launch Maintenance](#phase-18-post-launch-maintenance)

- [Project Milestones](#project-milestones)
- [Automated vs Manual Tasks](#automated-vs-manual-tasks)
- [Final Production-Readiness Checklist](#final-production-readiness-checklist)

---

## Phase 0: Key Concepts

Read this once. Mixing these terms up is the most common source of launch-day confusion.

| Term                    | What it is                                                                     | Example                                                  |
| ----------------------- | ------------------------------------------------------------------------------ | -------------------------------------------------------- |
| **Local development**   | The app running on your own machine for building and debugging.                | `pnpm dev` → `http://localhost:3000`                     |
| **Hosting**             | A server or platform that runs your app 24/7 on the internet.                  | Vercel, Railway, Render, a VPS                           |
| **Deployment**          | The act of building your code and putting a new version onto the host.         | `git push` triggers a build on Vercel                    |
| **Domain registration** | Renting a human-readable name for a yearly fee.                                | Buying `clientsite.com` at a registrar                   |
| **DNS configuration**   | Records that tell the internet which server a domain points to.                | An `A` record pointing `clientsite.com` to the host's IP |
| **SSL/TLS**             | The certificate that makes `https://` work. Most hosts issue it automatically. | Let's Encrypt via your host                              |

Buying a domain does not put your site online, and deploying does not give you a domain. You need all three: a **host** running the app, a **domain** you own, and **DNS** connecting them.

### The Three Environments

| Environment     | Purpose                                           | Database                                     | Who uses it            | URL example              |
| --------------- | ------------------------------------------------- | -------------------------------------------- | ---------------------- | ------------------------ |
| **Development** | Building features                                 | Local DB or a dev cloud DB with fake data    | Developers             | `localhost:3000`         |
| **Staging**     | Testing the exact production build before release | Separate DB with realistic (anonymised) data | Developers, QA, client | `staging.clientsite.com` |
| **Production**  | The live site                                     | Production DB with real data, backed up      | Real users             | `clientsite.com`         |

Rules:

- Each environment has **its own database and its own secrets**. Never point staging or dev at the production database.
- Staging should mirror production settings (same Node version, same build command, same hosting type).
- Code moves one way: **dev → staging → production**. Never hot-fix production directly.
- **(Optional) Preview environments:** per-pull-request deployments (Vercel and Netlify do this automatically). Useful for client review; make sure previews use a non-production database.

---

## Phase 1: Project Planning

### Objectives and Outcomes

- A clear, written agreement on what you are building, for whom, and what "done" means.
- **Outcome:** a signed-off scope document, a sitemap, a prioritised feature list, and a timeline.

### Step-by-Step

1. **Run a discovery meeting.** Ask about business goals, target audience, competitors, existing brand assets, content owners, launch deadline, budget, and who approves work.
2. **Define success metrics.** Example: "Increase contact form submissions by 30% within 6 months" or "Editors can publish a blog post without a developer."
3. **Identify users.** Write 2–4 short personas: who they are, what device they use, what they need from the site.
   - Also list **internal users**: content editors and admins who will use Payload.
4. **Write user stories** in the format _As a [user], I want [action], so that [benefit]_, with acceptance criteria:
   ```text
   As a visitor, I want to filter services by category, so that I find relevant offers quickly.
   Acceptance criteria:
   - Category filter is visible on /services
   - Selecting a category updates the list without a full page reload
   - The selected filter is reflected in the URL (?category=design)
   ```
5. **Create the sitemap** (page list and hierarchy):
   ```text
   /
   ├── /about
   ├── /services
   │   └── /services/[slug]
   ├── /blog
   │   └── /blog/[slug]
   ├── /contact
   ├── /privacy-policy
   └── /terms
   ```
6. **Prioritise features with MoSCoW:** Must have, Should have, Could have, Won't have (this release).
7. **Decide non-functional requirements now:** languages (i18n), accessibility target (WCAG 2.2 AA), performance targets (Core Web Vitals "Good"), browsers to support, SEO needs, legal pages, cookie consent, data privacy rules (GDPR, India's DPDP Act, etc.).
8. **Plan content.** Who writes it, when it is delivered, and in what format. Late content is the #1 cause of launch delays.
9. **Estimate and plan milestones** (see [Project Milestones](#project-milestones)).
10. **Get written sign-off** on scope. Agree how change requests are handled.

### Recommended Tools

- Docs: Notion, Google Docs, Confluence
- Tasks: Linear, Jira, GitHub Projects, Trello
- Sitemaps and flows: FigJam, Miro, Whimsical

### Common Mistakes

| Mistake                                                 | How to avoid it                                                |
| ------------------------------------------------------- | -------------------------------------------------------------- |
| Starting to code before scope is agreed                 | Get a signed scope document first                              |
| Forgetting editors as users                             | Write user stories for the CMS too                             |
| Treating SEO, accessibility, and performance as "later" | Add them as requirements in this phase                         |
| Waiting for content until the end                       | Set content delivery dates in the plan                         |
| No process for change requests                          | Agree that new requests are estimated and scheduled separately |

### Checklist

- [ ] Business goals and success metrics written down
- [ ] Personas for visitors and editors
- [ ] User stories with acceptance criteria
- [ ] Sitemap approved
- [ ] Features prioritised (MoSCoW)
- [ ] Non-functional requirements agreed (a11y, performance, SEO, i18n, browsers, legal)
- [ ] Content owners and delivery dates set
- [ ] Timeline and milestones agreed
- [ ] Scope signed off by the client

---

## Phase 2: UI/UX Design

### Objectives and Outcomes

- Decide what every page looks like and how it behaves on every screen size before you code it.
- **Outcome:** approved wireframes, high-fidelity designs for mobile and desktop, a design system, and a clean handoff.

### Step-by-Step

1. **Wireframes (low fidelity).** Grey boxes only: layout, content order, navigation. Do mobile first, then desktop. Get approval before adding colour.
2. **Define the design system (design tokens):**
   - **Colour palette:** primary, secondary, neutral greys, and semantic colours (success, warning, error, info). Check every text/background pair for contrast (4.5:1 for normal text, 3:1 for large text and UI components).
   - **Typography:** 1–2 font families, a type scale (e.g. 12/14/16/18/20/24/30/36/48 px), line heights, font weights. Body text at least 16 px.
   - **Spacing scale:** e.g. multiples of 4 px (4, 8, 12, 16, 24, 32, 48, 64).
   - **Breakpoints:** e.g. `sm 640`, `md 768`, `lg 1024`, `xl 1280`, `2xl 1536` (Tailwind defaults).
   - **Radius, shadows, borders, icon set.**
3. **Design components with all states:** default, hover, focus, active, disabled, loading, error, empty. Focus states must be clearly visible.
4. **High-fidelity mockups** for each unique page template at mobile (≈375 px) and desktop (≈1440 px).
5. **Clickable prototype (Optional).** Useful when the client struggles to imagine flows, or for complex forms.
6. **Usability check.** Show the prototype to 3–5 people from the target audience and watch them complete key tasks.
7. **Design handoff:**
   - Name components and tokens to match code (e.g. `Button/Primary/Large`).
   - Export icons as SVG and images in modern formats.
   - Document interactions (animations, transitions, what happens on submit).
   - Use Figma Dev Mode or variables so developers can read exact values.

### Recommended Tools

- Figma (design, prototyping, Dev Mode, variables)
- Contrast checking: WebAIM Contrast Checker, Figma plugins like Stark
- Inspiration: Mobbin, Dribbble (for patterns, not copying)

### Example: Design Tokens as CSS Variables

```css
:root {
  --color-primary: #1d4ed8;
  --color-primary-foreground: #ffffff;
  --color-text: #111827;
  --color-muted: #6b7280;
  --color-bg: #ffffff;
  --color-error: #b91c1c;
  --font-sans: "Inter", system-ui, sans-serif;
  --radius-md: 0.5rem;
  --space-4: 1rem;
}
@media (prefers-color-scheme: dark) {
  :root {
    --color-text: #f9fafb;
    --color-bg: #0b0f19;
  }
}
```

### Common Mistakes

| Mistake                                                 | How to avoid it                                      |
| ------------------------------------------------------- | ---------------------------------------------------- |
| Designing desktop only                                  | Design mobile first; approve both sizes              |
| No focus, error, empty, or loading states               | Require every component to show all states           |
| Low-contrast grey text                                  | Check contrast while designing, not after            |
| Too many font sizes and colours                         | Use a fixed token scale                              |
| Designs with fake content that doesn't fit real content | Use realistic content lengths, including long titles |

### Checklist

- [ ] Wireframes approved
- [ ] Design tokens defined (colour, type, spacing, breakpoints)
- [ ] All colour pairs pass WCAG AA contrast
- [ ] Components designed with all states
- [ ] Mobile and desktop mockups approved for every template
- [ ] Assets exported (SVG icons, optimised images)
- [ ] Handoff notes written

---

## Phase 3: Project Setup

### Objectives and Outcomes

- A clean, consistent, version-controlled project that every developer can run with one command.
- **Outcome:** a Next.js + TypeScript + Payload repository with linting, formatting, Git hooks, environment variables, and a README.

### Choosing the Architecture

Decide this first. For most marketing sites, blogs, and business websites, **one Next.js app with Payload installed inside it** is enough.

| Architecture                                           | When to use it                                                                                                                                               |
| ------------------------------------------------------ | ------------------------------------------------------------------------------------------------------------------------------------------------------------ |
| **Next.js + Payload in one app** (recommended default) | Marketing sites, blogs, company sites, small e-commerce. Payload 3 runs inside the Next.js App Router; one codebase, one deployment.                         |
| **Next.js frontend + separate Express/Node API**       | You need long-running processes, WebSockets, heavy background jobs, an existing Express API, or a mobile app sharing the same API with complex custom logic. |
| **Monorepo (Turborepo / pnpm workspaces)** (Optional)  | Several apps (website, admin, API, mobile) sharing types and UI packages.                                                                                    |

**When you do NOT need Express:**

- Form submissions, webhooks, and small APIs → Next.js **Route Handlers** (`app/api/.../route.ts`) or **Server Actions**.
- CRUD for content, auth, uploads, and access control → **Payload** already provides REST, GraphQL, and a Local API.

**When Express (or another Node server) is justified:**

- Persistent WebSocket connections (chat, live dashboards)
- Long-running or scheduled jobs that exceed serverless time limits (or use Payload Jobs Queue / a worker)
- Integrating a legacy Express API you must keep
- Fine-grained control over the HTTP server (custom proxies, streaming protocols)

### Step-by-Step

1. **Install prerequisites**
   - Node.js LTS (use `nvm` or `fnm` to manage versions)
   - pnpm (`corepack enable` then `corepack prepare pnpm@latest --activate`)
   - Git, VS Code (or your editor), Docker (Optional, for a local database)

2. **Create the project with Payload + Next.js**

   ```bash
   # Option A: Payload template (includes Next.js)
   npx create-payload-app@latest my-site
   # Choose a template (e.g. "website" or "blank") and a database (MongoDB or Postgres)

   # Option B: Next.js first, then add Payload into it
   npx create-next-app@latest my-site --typescript --eslint --tailwind --app --src-dir
   cd my-site
   npx create-payload-app@latest   # run inside the existing Next.js app
   ```

3. **Pin the Node version**

   ```bash
   node -v > .nvmrc
   ```

   And in `package.json`:

   ```json
   {
     "engines": { "node": ">=20.9.0" },
     "packageManager": "pnpm@9.15.0"
   }
   ```

   (Match the versions to what you actually installed.)

4. **TypeScript strict mode** in `tsconfig.json`:

   ```json
   {
     "compilerOptions": {
       "strict": true,
       "noUncheckedIndexedAccess": true,
       "baseUrl": ".",
       "paths": {
         "@/*": ["./src/*"],
         "@payload-config": ["./src/payload.config.ts"]
       }
     }
   }
   ```

5. **ESLint and Prettier**

   ```bash
   pnpm add -D prettier eslint-config-prettier prettier-plugin-tailwindcss
   ```

   `.prettierrc`:

   ```json
   {
     "semi": false,
     "singleQuote": true,
     "trailingComma": "all",
     "printWidth": 100,
     "plugins": ["prettier-plugin-tailwindcss"]
   }
   ```

   `package.json` scripts:

   ```json
   {
     "scripts": {
       "dev": "next dev",
       "build": "next build",
       "start": "next start",
       "lint": "eslint .",
       "typecheck": "tsc --noEmit",
       "format": "prettier --write .",
       "format:check": "prettier --check .",
       "test": "vitest run",
       "test:e2e": "playwright test",
       "generate:types": "payload generate:types"
     }
   }
   ```

   Note: recent Next.js versions use the ESLint CLI directly (`eslint .`) with a flat config (`eslint.config.mjs`) rather than `next lint`.

6. **Git setup**

   ```bash
   git init
   git add .
   git commit -m "chore: initial project setup"
   git branch -M main
   git remote add origin git@github.com:your-org/my-site.git
   git push -u origin main
   ```

   `.gitignore` must include:

   ```text
   node_modules
   .next
   .env
   .env*.local
   /media
   coverage
   playwright-report
   ```

7. **Git hooks (Optional but recommended)** to stop bad commits:

   ```bash
   pnpm add -D husky lint-staged
   pnpm exec husky init
   echo "pnpm exec lint-staged" > .husky/pre-commit
   ```

   `package.json`:

   ```json
   {
     "lint-staged": {
       "*.{ts,tsx}": ["eslint --fix", "prettier --write"],
       "*.{json,md,css}": ["prettier --write"]
     }
   }
   ```

8. **Environment variables**
   - `.env` (never committed) holds real values.
   - `.env.example` (committed) documents every variable with fake values.

   `.env.example`:

   ```bash
   # App
   NEXT_PUBLIC_SITE_URL=http://localhost:3000

   # Payload
   PAYLOAD_SECRET=replace-with-a-long-random-string
   DATABASE_URI=mongodb://127.0.0.1:27017/my-site
   # Postgres example: postgres://user:password@localhost:5432/my_site

   # Email (SMTP or provider API)
   SMTP_HOST=
   SMTP_USER=
   SMTP_PASS=

   # Media storage (production)
   S3_BUCKET=
   S3_REGION=
   S3_ACCESS_KEY_ID=
   S3_SECRET_ACCESS_KEY=

   # Monitoring
   SENTRY_DSN=
   ```

   Rules:
   - Only variables prefixed `NEXT_PUBLIC_` reach the browser. **Never** put secrets in them.
   - Generate secrets with `openssl rand -base64 32`.
   - Validate env vars at startup (Optional, recommended) with `zod` or `@t3-oss/env-nextjs` so a missing value fails the build instead of breaking production.

9. **Folder structure** (example):

   ```text
   src/
   ├── app/
   │   ├── (frontend)/        # public website routes
   │   └── (payload)/         # Payload admin + API routes
   ├── collections/           # Payload collections
   ├── globals/               # Payload globals (Header, Footer, Settings)
   ├── blocks/                # page-builder blocks
   ├── components/            # shared React components
   │   └── ui/                # primitives: Button, Input, Card
   ├── lib/                   # utilities, data fetching, validation
   ├── hooks/
   ├── styles/
   └── payload.config.ts
   ```

10. **Write the README:** how to install, required env vars, how to run, how to seed data, how to deploy.

### Recommended Tools

- Node.js LTS, pnpm, TypeScript, ESLint, Prettier, Husky, lint-staged
- Tailwind CSS (or CSS Modules) for styling
- Docker for local MongoDB/Postgres (Optional)

### Docs

- Next.js: https://nextjs.org/docs
- Payload installation: https://payloadcms.com/docs/getting-started/installation
- TypeScript: https://www.typescriptlang.org/docs/
- ESLint: https://eslint.org/docs/latest/

### Common Mistakes

| Mistake                                        | How to avoid it                                                   |
| ---------------------------------------------- | ----------------------------------------------------------------- |
| Committing `.env`                              | Add to `.gitignore` before the first commit; use `.env.example`   |
| Secrets in `NEXT_PUBLIC_` variables            | Only public, non-sensitive values get that prefix                 |
| Different Node versions across machines and CI | `.nvmrc` + `engines` field                                        |
| Adding Express "just in case"                  | Start with Next.js + Payload; add a server only for a proven need |
| Mixing npm, yarn, and pnpm                     | Pick one, commit its lockfile, set `packageManager`               |

### Checklist

- [ ] Architecture decided and written in the README
- [ ] Next.js + Payload running locally
- [ ] TypeScript strict mode on
- [ ] `lint`, `typecheck`, `format:check` scripts pass
- [ ] Git repo created, first commit pushed, `.env` ignored
- [ ] `.env.example` documents every variable
- [ ] Node version pinned
- [ ] README explains setup in under 5 steps

---

## Phase 4: Frontend Development

### Objectives and Outcomes

- Build the UI from the designs as reusable, accessible, responsive, typed components.
- **Outcome:** all page templates built, connected to real or mock data, working on all target screen sizes.

### Step-by-Step

1. **Build primitives first** (`components/ui`): Button, Link, Input, Textarea, Select, Checkbox, Card, Badge, Container, Heading. Each one:
   - uses design tokens,
   - supports all states (hover, focus-visible, disabled, loading, error),
   - forwards props and `ref` where needed,
   - is accessible by default (native elements first).

   ```tsx
   // src/components/ui/Button.tsx
   import { cn } from "@/lib/utils";
   type Props = React.ButtonHTMLAttributes<HTMLButtonElement> & {
     variant?: "primary" | "secondary";
     loading?: boolean;
   };
   export function Button({
     variant = "primary",
     loading,
     disabled,
     children,
     className,
     ...props
   }: Props) {
     return (
       <button
         className={cn("btn", `btn-${variant}`, className)}
         disabled={disabled || loading}
         aria-busy={loading || undefined}
         {...props}
       >
         {loading ? "Loading…" : children}
       </button>
     );
   }
   ```

   (Optional) Use a headless/accessible component library such as shadcn/ui, Radix UI, or React Aria for complex widgets (dialogs, menus, comboboxes) instead of building them from scratch.

2. **Layouts.** Create the root layout and shared layouts in the App Router:

   ```tsx
   // src/app/(frontend)/layout.tsx
   export default function RootLayout({
     children,
   }: {
     children: React.ReactNode;
   }) {
     return (
       <html lang="en">
         <body>
           <a href="#main" className="skip-link">
             Skip to content
           </a>
           <Header />
           <main id="main">{children}</main>
           <Footer />
         </body>
       </html>
     );
   }
   ```

3. **Routing** (App Router file conventions):

   ```text
   app/(frontend)/page.tsx                 → /
   app/(frontend)/blog/page.tsx            → /blog
   app/(frontend)/blog/[slug]/page.tsx     → /blog/my-post
   app/(frontend)/loading.tsx              → loading UI
   app/(frontend)/error.tsx                → error boundary (client component)
   app/(frontend)/not-found.tsx            → 404 page
   ```

4. **Server vs Client Components.**
   - Default to **Server Components** (fetch data, render HTML, no JS shipped).
   - Add `'use client'` only for interactivity: state, effects, event handlers, browser APIs.
   - Keep client components small and push them to the leaves of the tree.

5. **Data fetching.** In Server Components, fetch directly from Payload's Local API (no HTTP round trip):

   ```tsx
   import { getPayload } from "payload";
   import config from "@payload-config";
   import { notFound } from "next/navigation";

   export default async function PostPage({
     params,
   }: {
     params: Promise<{ slug: string }>;
   }) {
     const { slug } = await params;
     const payload = await getPayload({ config });
     const { docs } = await payload.find({
       collection: "posts",
       where: { slug: { equals: slug } },
       limit: 1,
     });
     const post = docs[0];
     if (!post) notFound();
     return (
       <article>
         <h1>{post.title}</h1>
       </article>
     );
   }
   ```

6. **Responsive design.** Mobile-first CSS; test at 320, 375, 768, 1024, 1440 px. Use relative units, `max-width: 100%` on media, and avoid fixed heights for text containers.

7. **Forms.**
   - Use `react-hook-form` + `zod` (client) and validate again on the server (never trust the client).
   - Or use Server Actions with `useActionState` for progressive enhancement.
   - Every input has a visible `<label>`, error messages are linked with `aria-describedby`, and submit buttons show a loading state.

   ```tsx
   const schema = z.object({
     name: z.string().min(2, "Name is too short"),
     email: z.string().email("Enter a valid email"),
     message: z.string().min(10, "Message is too short"),
   });
   ```

8. **Loading, empty, and error states.** Every data view needs: loading (`loading.tsx` or Suspense skeletons), empty ("No posts yet"), error (`error.tsx` with a retry button), and not found (`notFound()`).

9. **State management.** Use the simplest option that works:
   | Need | Use |
   | --- | --- |
   | Server data | Server Components; TanStack Query only for heavy client-side fetching |
   | UI state in one component | `useState` / `useReducer` |
   | Shared UI state (theme, cart) | React Context, or Zustand if it grows |
   | Filters, search, pagination | URL search params (shareable and SEO-friendly) |
   | Form state | react-hook-form |

10. **Images and fonts:** use `next/image` and `next/font` from the start (details in Phase 10).

11. **(Optional) Internationalisation:** if multiple languages are in scope, set up `next-intl` or `react-i18next` plus Payload localization now, not after launch.

### Recommended Tools

- React, Next.js App Router, TypeScript, Tailwind CSS
- shadcn/ui, Radix UI, or React Aria (Optional)
- react-hook-form, zod
- Storybook (Optional, useful for larger design systems)

### Docs

- React: https://react.dev
- App Router: https://nextjs.org/docs/app
- Server and Client Components: https://nextjs.org/docs/app/getting-started/server-and-client-components

### Common Mistakes

| Mistake                                     | How to avoid it                                  |
| ------------------------------------------- | ------------------------------------------------ |
| `'use client'` on whole pages               | Keep client components small and at the leaves   |
| `div` with `onClick` instead of `button`    | Use native elements                              |
| Validating only on the client               | Always validate again on the server              |
| Forgetting loading, empty, and error states | Add them to the definition of done for each view |
| Global state for everything                 | Prefer server data and URL params                |
| Copy-pasting components                     | Extract reusable primitives early                |

### Checklist

- [ ] UI primitives built with all states
- [ ] Layouts, header, footer, and navigation done (including mobile menu)
- [ ] All page templates built and match designs
- [ ] Responsive at 320–1440 px+
- [ ] Forms validate on client and server with accessible errors
- [ ] Loading, empty, error, and 404 states exist
- [ ] No unnecessary client components
- [ ] `lint` and `typecheck` pass

---

## Phase 5: Backend Development

### Objectives and Outcomes

- Server-side logic that is validated, secure, and predictable.
- **Outcome:** documented API endpoints/actions with validation, authentication, authorization, and consistent error handling.

### Decide Where Backend Logic Lives

| Task                                                   | Best place                                          |
| ------------------------------------------------------ | --------------------------------------------------- |
| Content CRUD, auth, uploads, access rules              | **Payload** (collections, access control, hooks)    |
| Form submissions, small mutations                      | **Server Actions**                                  |
| Webhooks, public JSON endpoints, third-party callbacks | **Next.js Route Handlers**                          |
| Custom endpoint tied to a collection                   | **Payload custom endpoints**                        |
| Background jobs, scheduled tasks                       | **Payload Jobs Queue**, a cron service, or a worker |
| WebSockets, long-running processes, legacy APIs        | **Separate Node.js/Express service**                |

### Step-by-Step

1. **Design the API before coding.** List each endpoint: method, path, input, output, auth requirement, errors.

   ```text
   POST /api/contact
     Auth: none (rate limited)
     Body: { name: string, email: string, message: string }
     201: { ok: true }
     400: { error: "VALIDATION_ERROR", details: [...] }
     429: { error: "RATE_LIMITED" }
   ```

   Use REST conventions: nouns in paths, correct methods (GET read, POST create, PATCH update, DELETE remove), correct status codes.

2. **Validate every input on the server** with `zod`:

   ```ts
   // src/app/api/contact/route.ts
   import { NextResponse } from "next/server";
   import { z } from "zod";

   const ContactSchema = z.object({
     name: z.string().trim().min(2).max(100),
     email: z.string().trim().email().max(200),
     message: z.string().trim().min(10).max(5000),
   });

   export async function POST(req: Request) {
     const json = await req.json().catch(() => null);
     const parsed = ContactSchema.safeParse(json);
     if (!parsed.success) {
       return NextResponse.json(
         { error: "VALIDATION_ERROR", details: parsed.error.flatten() },
         { status: 400 },
       );
     }
     // save to Payload, send email, etc.
     return NextResponse.json({ ok: true }, { status: 201 });
   }
   ```

3. **Server Action example:**

   ```ts
   "use server";
   export async function subscribe(prevState: unknown, formData: FormData) {
     const result = z.string().email().safeParse(formData.get("email"));
     if (!result.success) return { ok: false, error: "Enter a valid email" };
     // save subscriber
     return { ok: true };
   }
   ```

   Server Actions are public HTTP endpoints. Validate input and check authorization inside every action.

4. **Authentication.**
   - For editors/admins and site members, use **Payload's built-in auth** (`auth: true` on a `users` collection): hashed passwords, HTTP-only cookies, login lockout, password reset.
   - For social login or complex identity needs (Optional): Auth.js, Clerk, or another provider.
   - Store sessions in **HTTP-only, Secure, SameSite** cookies, not localStorage.

5. **Authorization.** Check _what_ a logged-in user may do, on the server, every time. In Payload, use access control functions (Phase 6). In custom routes, check the user and role before acting.

6. **Consistent error handling.**
   - Return a stable error shape: `{ error: 'CODE', message?: string, details?: unknown }`.
   - Never leak stack traces or database errors to the client in production.
   - Log the full error server-side (Phase 13).

7. **Emails and third-party services.** Wrap external calls with timeouts and retries; never block a user response on a slow third party if you can queue it.

8. **(Optional) Express service** when justified:

   ```bash
   mkdir api && cd api && pnpm init
   pnpm add express helmet cors express-rate-limit zod
   pnpm add -D typescript tsx @types/express @types/node
   ```

   ```ts
   // api/src/server.ts
   import express from "express";
   import helmet from "helmet";
   import cors from "cors";
   import rateLimit from "express-rate-limit";

   const app = express();
   app.use(helmet());
   app.use(cors({ origin: process.env.ALLOWED_ORIGIN, credentials: true }));
   app.use(express.json({ limit: "100kb" }));
   app.use(rateLimit({ windowMs: 15 * 60 * 1000, limit: 100 }));

   app.get("/health", (_req, res) => res.json({ ok: true }));

   app.use(
     (
       err: Error,
       _req: express.Request,
       res: express.Response,
       _next: express.NextFunction,
     ) => {
       console.error(err);
       res.status(500).json({ error: "INTERNAL_ERROR" });
     },
   );

   app.listen(process.env.PORT ?? 4000);
   ```

   Note: Payload 3 runs inside Next.js; you do not need Express to run Payload.

### Recommended Tools

- zod (validation), Payload auth, Resend / Nodemailer / Postmark (email)
- Express, helmet, cors, express-rate-limit (only for a separate service)
- Bruno, Postman, or Insomnia for testing APIs

### Docs

- Route Handlers: https://nextjs.org/docs/app/getting-started/route-handlers-and-middleware
- Server Actions: https://nextjs.org/docs/app/getting-started/updating-data
- Payload authentication: https://payloadcms.com/docs/authentication/overview
- Express: https://expressjs.com/

### Common Mistakes

| Mistake                           | How to avoid it                                      |
| --------------------------------- | ---------------------------------------------------- |
| Trusting client-side validation   | Validate on the server with a schema                 |
| Checking auth only in the UI      | Check authorization in every server function         |
| Leaking error details             | Generic messages to clients, full logs on the server |
| Tokens in localStorage            | Use HTTP-only cookies                                |
| Building custom auth from scratch | Use Payload auth or a proven provider                |

### Checklist

- [ ] Every endpoint/action documented (input, output, auth, errors)
- [ ] All input validated on the server
- [ ] Authentication uses secure cookies
- [ ] Authorization checked server-side for every protected operation
- [ ] Consistent error format; no stack traces leak
- [ ] External calls have timeouts
- [ ] Express added only if a documented need exists

---

## Phase 6: CMS Integration (Payload CMS)

### Objectives and Outcomes

- Editors can manage all content safely without a developer.
- **Outcome:** collections, globals, blocks, access control, media handling, drafts/preview, and an editor guide.

### Key Concepts

| Concept            | What it is                                           | Examples                                                 |
| ------------------ | ---------------------------------------------------- | -------------------------------------------------------- |
| **Collection**     | A group of documents of the same type                | Pages, Posts, Services, Users, Media                     |
| **Global**         | A single document                                    | Header, Footer, Site Settings                            |
| **Field**          | A piece of data on a document                        | text, richText, upload, relationship, array, blocks      |
| **Block**          | A reusable section editors can stack on a page       | Hero, Feature Grid, CTA, Testimonials                    |
| **Access control** | Functions deciding who can read/create/update/delete | Public can read published posts; only editors can update |
| **Hooks**          | Functions that run before/after operations           | Revalidate the page cache after publishing               |

### Step-by-Step

1. **Configure Payload** (`src/payload.config.ts`):

   ```ts
   import { buildConfig } from "payload";
   import { mongooseAdapter } from "@payloadcms/db-mongodb";
   // or: import { postgresAdapter } from '@payloadcms/db-postgres'
   import { lexicalEditor } from "@payloadcms/richtext-lexical";
   import { Users } from "./collections/Users";
   import { Media } from "./collections/Media";
   import { Pages } from "./collections/Pages";
   import { Posts } from "./collections/Posts";
   import { Header } from "./globals/Header";
   import { Footer } from "./globals/Footer";

   export default buildConfig({
     secret: process.env.PAYLOAD_SECRET!,
     serverURL: process.env.NEXT_PUBLIC_SITE_URL,
     admin: { user: Users.slug },
     collections: [Users, Media, Pages, Posts],
     globals: [Header, Footer],
     editor: lexicalEditor(),
     db: mongooseAdapter({ url: process.env.DATABASE_URI! }),
     typescript: { outputFile: "src/payload-types.ts" },
   });
   ```

2. **Users with roles:**

   ```ts
   // src/collections/Users.ts
   import type { CollectionConfig } from "payload";
   export const Users: CollectionConfig = {
     slug: "users",
     auth: true,
     admin: { useAsTitle: "email" },
     access: {
       read: ({ req: { user } }) => Boolean(user),
       create: ({ req: { user } }) => user?.role === "admin",
       update: ({ req: { user }, id }) =>
         user?.role === "admin" || user?.id === id,
       delete: ({ req: { user } }) => user?.role === "admin",
     },
     fields: [
       {
         name: "role",
         type: "select",
         options: ["admin", "editor"],
         defaultValue: "editor",
         required: true,
         access: { update: ({ req: { user } }) => user?.role === "admin" },
       },
     ],
   };
   ```

3. **Content collection with drafts, slug, SEO, and revalidation:**

   ```ts
   // src/collections/Posts.ts
   import type { CollectionConfig } from "payload";
   import { revalidatePath } from "next/cache";

   export const Posts: CollectionConfig = {
     slug: "posts",
     admin: {
       useAsTitle: "title",
       defaultColumns: ["title", "status", "updatedAt"],
     },
     versions: { drafts: true },
     access: {
       read: ({ req: { user } }) =>
         user ? true : { _status: { equals: "published" } },
       create: ({ req: { user } }) => Boolean(user),
       update: ({ req: { user } }) => Boolean(user),
       delete: ({ req: { user } }) => user?.role === "admin",
     },
     fields: [
       { name: "title", type: "text", required: true },
       {
         name: "slug",
         type: "text",
         required: true,
         unique: true,
         index: true,
       },
       { name: "excerpt", type: "textarea", maxLength: 300 },
       { name: "coverImage", type: "upload", relationTo: "media" },
       { name: "content", type: "richText" },
       { name: "author", type: "relationship", relationTo: "users" },
       { name: "publishedAt", type: "date" },
     ],
     hooks: {
       afterChange: [
         ({ doc }) => {
           revalidatePath(`/blog/${doc.slug}`);
           revalidatePath("/blog");
         },
       ],
     },
   };
   ```

4. **Media uploads:**

   ```ts
   // src/collections/Media.ts
   export const Media: CollectionConfig = {
     slug: "media",
     access: { read: () => true, create: ({ req: { user } }) => Boolean(user) },
     upload: {
       mimeTypes: ["image/*", "application/pdf"],
       imageSizes: [
         { name: "thumbnail", width: 400 },
         { name: "card", width: 768 },
         { name: "hero", width: 1920 },
       ],
     },
     fields: [{ name: "alt", type: "text", required: true }],
   };
   ```

   - Make `alt` required so editors can't skip it (accessibility + SEO).
   - In production on serverless hosts, use a **storage adapter** (S3, Cloudflare R2, Vercel Blob, etc.). The local filesystem is not persistent there.

5. **Globals** for site-wide content (Header nav, Footer links, Site Settings like default SEO image, social links, contact details).

6. **Blocks / page builder (Optional but common):** define blocks (Hero, Content, CTA, MediaBlock, FAQ) and a `layout` field of type `blocks` on Pages. Each block maps to one React component.

7. **Relationships:** use `relationship` fields (post → author, post → categories). Control how much related data is returned with `depth` in queries to avoid huge payloads.

8. **Live preview and draft preview** so editors see changes before publishing (Payload Live Preview + Next.js Draft Mode).

9. **(Optional) Localization:** enable `localization` in the config and `localized: true` on text fields if the site is multilingual.

10. **(Optional) Plugins:** `@payloadcms/plugin-seo`, `plugin-redirects`, `plugin-form-builder`, `plugin-search`, `plugin-nested-docs`.

11. **Generate types after every schema change:**

    ```bash
    pnpm payload generate:types
    ```

12. **Seed script** with realistic sample data so every developer and staging environment starts with the same content.

13. **Write an editor guide:** how to log in, create a page, add images with alt text, preview, publish, and what not to touch.

### Docs

- Payload docs: https://payloadcms.com/docs
- Collections: https://payloadcms.com/docs/configuration/collections
- Access control: https://payloadcms.com/docs/access-control/overview
- Uploads and storage adapters: https://payloadcms.com/docs/upload/overview
- Live Preview: https://payloadcms.com/docs/live-preview/overview

### Common Mistakes

| Mistake                                        | How to avoid it                                  |
| ---------------------------------------------- | ------------------------------------------------ |
| Default open access (anyone can read drafts)   | Write explicit access rules for every collection |
| Hardcoding content that editors need to change | Move it into a global or collection              |
| Optional alt text                              | Make `alt` required                              |
| Local file storage on serverless hosting       | Use an S3/R2/Blob storage adapter                |
| Pages not updating after publishing            | Revalidate paths/tags in `afterChange` hooks     |
| Huge responses from deep relationships         | Set `depth` and `select` in queries              |

### Checklist

- [ ] All collections and globals created with explicit access control
- [ ] Roles defined (admin, editor) and tested
- [ ] Drafts, preview, and publish workflow work
- [ ] Media uploads with required alt text and image sizes
- [ ] Production storage adapter configured
- [ ] Cache revalidation on publish works
- [ ] Types generated and committed
- [ ] Seed script works on a fresh database
- [ ] Editor guide written and tested by a non-developer

---

## Phase 7: Database Integration

### Objectives and Outcomes

- A reliable, secure, backed-up database with a schema that fits the content.
- **Outcome:** chosen database, schema and indexes, migration process, backup and restore tested, secure access.

### Step-by-Step

1. **Choose the database.** Payload supports MongoDB, PostgreSQL, and SQLite through adapters.
   | Option | Choose when |
   | --- | --- |
   | **PostgreSQL** | Relational data, strong consistency, reporting, many relationships. Good default for most projects. |
   | **MongoDB** | Flexible, deeply nested documents; team already uses Mongo. |
   | **SQLite** (Optional) | Small sites, prototypes, or local development. |
   Managed hosts: Neon, Supabase, AWS RDS, DigitalOcean (Postgres); MongoDB Atlas (Mongo).

2. **Design the schema from your content model.** In Payload, your collections _are_ the schema. Before coding, list entities, fields, and relationships:

   ```text
   Users (1) ──< Posts >── (n) Categories
   Posts (n) ──> (1) Media (coverImage)
   Pages: blocks[] (Hero | Content | CTA)
   ```

3. **Indexes.** Add `index: true` to fields you filter or sort by (slug, publishedAt, status). Add `unique: true` where duplicates are invalid (slug, email).

4. **Validation at the data layer.** Use `required`, `min`/`max`, `maxLength`, `unique`, and custom `validate` functions on Payload fields, in addition to API-level validation.

5. **Migrations** (required for Postgres/SQLite; recommended for structural MongoDB changes):

   ```bash
   pnpm payload migrate:create add-post-excerpt   # create a migration after schema changes
   pnpm payload migrate                           # run pending migrations
   pnpm payload migrate:status                    # see what has run
   ```

   - Commit migration files to Git.
   - Run migrations in CI/CD **before** the new app version starts serving traffic.
   - In development, Postgres can auto-push schema changes; in staging and production always use migrations.

6. **Local database.** Use Docker for parity:

   ```yaml
   # docker-compose.yml
   services:
     postgres:
       image: postgres:16
       environment:
         POSTGRES_USER: dev
         POSTGRES_PASSWORD: dev
         POSTGRES_DB: my_site
       ports: ["5432:5432"]
       volumes: ["pgdata:/var/lib/postgresql/data"]
   volumes:
     pgdata:
   ```

7. **Backups.**
   - Enable automatic daily backups (and point-in-time recovery if available) on the managed database.
   - Keep at least 7–30 days of backups; store a copy outside the main provider for critical sites.
   - Back up media storage too (S3 versioning or replication).
   - **Test a restore** into a separate database. An untested backup is not a backup.

   ```bash
   # Postgres
   pg_dump "$DATABASE_URI" -Fc -f backup-$(date +%F).dump
   pg_restore -d "$RESTORE_DATABASE_URI" backup-2026-01-15.dump
   # MongoDB
   mongodump --uri="$DATABASE_URI" --out=backup-$(date +%F)
   mongorestore --uri="$RESTORE_DATABASE_URI" backup-2026-01-15
   ```

8. **Database security.**
   - Separate databases and credentials per environment.
   - Least-privilege database user for the app (no superuser).
   - Strong, generated passwords stored only in the host's secret manager.
   - Restrict network access (IP allow-list, private networking, or VPC).
   - Require TLS connections.
   - Never expose the database port to the public internet.

### Docs

- Payload database overview: https://payloadcms.com/docs/database/overview
- Payload migrations: https://payloadcms.com/docs/database/migrations
- PostgreSQL: https://www.postgresql.org/docs/
- MongoDB: https://www.mongodb.com/docs/

### Common Mistakes

| Mistake                                         | How to avoid it                          |
| ----------------------------------------------- | ---------------------------------------- |
| Staging using the production database           | Separate DB per environment              |
| Schema changes without migrations in production | Create, commit, and run migrations in CI |
| Backups never tested                            | Schedule a restore test                  |
| Database open to `0.0.0.0/0`                    | IP allow-lists or private networking     |
| No indexes on slugs and dates                   | Index fields used in `where` and `sort`  |

### Checklist

- [ ] Database chosen with a written reason
- [ ] Schema/relationships documented
- [ ] Indexes and unique constraints in place
- [ ] Migration workflow tested
- [ ] Automated backups on; restore tested
- [ ] Separate credentials per environment; TLS on; network restricted

---

## Phase 8: SEO Implementation

### Objectives and Outcomes

- Search engines can crawl, understand, and index every public page, and pages look good when shared.
- **Outcome:** metadata, canonical URLs, Open Graph, structured data, sitemap, robots.txt, redirects, and clean URLs.

### Step-by-Step

1. **Clean URL structure.** Lowercase, hyphenated, short, descriptive: `/blog/how-to-choose-a-web-host`. No IDs or query strings for content pages. Decide trailing slash policy and keep it consistent.

2. **Site-wide metadata** in the root layout:

   ```ts
   // src/app/(frontend)/layout.tsx
   import type { Metadata } from "next";
   export const metadata: Metadata = {
     metadataBase: new URL(process.env.NEXT_PUBLIC_SITE_URL!),
     title: { default: "Client Name", template: "%s | Client Name" },
     description:
       "Short, unique description of the business (150–160 characters).",
     openGraph: { type: "website", siteName: "Client Name", locale: "en_IN" },
     twitter: { card: "summary_large_image" },
   };
   ```

3. **Per-page metadata from the CMS:**

   ```ts
   export async function generateMetadata({
     params,
   }: {
     params: Promise<{ slug: string }>;
   }): Promise<Metadata> {
     const { slug } = await params;
     const post = await getPostBySlug(slug);
     if (!post) return {};
     return {
       title: post.meta?.title ?? post.title,
       description: post.meta?.description ?? post.excerpt,
       alternates: { canonical: `/blog/${slug}` },
       openGraph: {
         title: post.title,
         description: post.excerpt ?? undefined,
         type: "article",
         publishedTime: post.publishedAt ?? undefined,
         images: post.meta?.image
           ? [{ url: post.meta.image.url, width: 1200, height: 630 }]
           : undefined,
       },
     };
   }
   ```

   - Title: unique per page, about 50–60 characters, main keyword near the start.
   - Description: unique, about 150–160 characters, written for humans.
   - Canonical: every indexable page points to its preferred URL.
   - Open Graph image: 1200×630 px. (Optional) Generate dynamic OG images with `opengraph-image.tsx`.

4. **Structured data (JSON-LD):**

   ```tsx
   const jsonLd = {
     "@context": "https://schema.org",
     "@type": "Article",
     headline: post.title,
     datePublished: post.publishedAt,
     dateModified: post.updatedAt,
     author: { "@type": "Person", name: post.author?.name },
     image: post.coverImage?.url,
   };
   return (
     <>
       <script
         type="application/ld+json"
         dangerouslySetInnerHTML={{
           __html: JSON.stringify(jsonLd).replace(/</g, "\\u003c"),
         }}
       />
       <article>…</article>
     </>
   );
   ```

   Common types: `Organization` / `LocalBusiness` (home), `Article` / `BlogPosting`, `BreadcrumbList`, `FAQPage`, `Product`. Only mark up content visible on the page.

5. **XML sitemap:**

   ```ts
   // src/app/sitemap.ts
   import type { MetadataRoute } from "next";
   export default async function sitemap(): Promise<MetadataRoute.Sitemap> {
     const base = process.env.NEXT_PUBLIC_SITE_URL!;
     const posts = await getAllPublishedPosts();
     return [
       { url: base, lastModified: new Date() },
       { url: `${base}/about` },
       ...posts.map((p) => ({
         url: `${base}/blog/${p.slug}`,
         lastModified: p.updatedAt,
       })),
     ];
   }
   ```

6. **robots.txt:**

   ```ts
   // src/app/robots.ts
   import type { MetadataRoute } from "next";
   export default function robots(): MetadataRoute.Robots {
     const isProd =
       process.env.VERCEL_ENV === "production" ||
       process.env.APP_ENV === "production";
     return isProd
       ? {
           rules: { userAgent: "*", allow: "/", disallow: ["/admin", "/api"] },
           sitemap: `${process.env.NEXT_PUBLIC_SITE_URL}/sitemap.xml`,
         }
       : { rules: { userAgent: "*", disallow: "/" } }; // block staging and previews
   }
   ```

   Also add `robots: { index: false }` metadata or an `X-Robots-Tag: noindex` header on staging, and protect it with a password if possible.

7. **Semantic HTML.** One `<h1>` per page, logical heading order (h1 → h2 → h3), `<nav>`, `<main>`, `<article>`, `<footer>`, descriptive link text ("Read the pricing guide", not "Click here").

8. **Redirects.** When URLs change (or when migrating from an old site), add permanent (301/308) redirects:

   ```ts
   // next.config.ts
   async redirects() {
     return [{ source: '/old-blog/:slug', destination: '/blog/:slug', permanent: true }]
   }
   ```

   For editor-managed redirects, use `@payloadcms/plugin-redirects`. Pick one canonical host (`www` or non-`www`) and redirect the other.

9. **Images:** descriptive filenames and alt text; use `next/image`.

10. **Indexing.** After launch: verify the site in Google Search Console (and Bing Webmaster Tools), submit the sitemap, and use URL Inspection for key pages (Phase 17).

11. **(Optional) Multilingual SEO:** `hreflang` via `alternates.languages` when the site has multiple languages.

### Docs

- Next.js Metadata: https://nextjs.org/docs/app/getting-started/metadata-and-og-images
- Google Search Central: https://developers.google.com/search/docs
- Schema.org: https://schema.org
- Rich Results Test: https://search.google.com/test/rich-results

### Common Mistakes

| Mistake                                                    | How to avoid it                                                   |
| ---------------------------------------------------------- | ----------------------------------------------------------------- |
| Staging indexed by Google                                  | `noindex` + `Disallow: /` + password protection on non-production |
| Same title/description on every page                       | Generate per-page metadata from CMS fields                        |
| Launching with production `robots.txt` blocking everything | Verify robots.txt on launch day                                   |
| Changing URLs without redirects                            | Map old → new URLs and add 301s                                   |
| Multiple `<h1>`s or skipped heading levels                 | Check heading outline in QA                                       |
| JSON-LD that doesn't match visible content                 | Only mark up what users can see                                   |

### Checklist

- [ ] `metadataBase`, default title template, and description set
- [ ] Unique title, description, and canonical on every page
- [ ] Open Graph and Twitter cards with 1200×630 images
- [ ] JSON-LD valid in Rich Results Test
- [ ] `sitemap.xml` lists all public pages, no drafts or admin
- [ ] `robots.txt` correct per environment
- [ ] Redirects for any changed or old URLs; one canonical host
- [ ] Semantic HTML and heading order checked

---

## Phase 9: Accessibility

### Objectives and Outcomes

- Everyone, including people using keyboards, screen readers, zoom, or voice control, can use the site.
- **Outcome:** the site meets **WCAG 2.2 Level AA**, verified with automated tools and manual testing.

### WCAG in One Paragraph

WCAG is organised around four principles (**POUR**): content must be **Perceivable** (text alternatives, captions, contrast), **Operable** (keyboard access, enough time, no seizure risks), **Understandable** (readable, predictable, helpful errors), and **Robust** (works with assistive technologies). Level AA is the common legal and contractual target.

### Step-by-Step

1. **Semantic HTML first.** Native elements give you keyboard and screen-reader support for free.
   | Need | Use | Not |
   | --- | --- | --- |
   | Action | `<button>` | `<div onClick>` |
   | Navigation to a URL | `<a href>` / `next/link` | `<span onClick={router.push}>` |
   | Page regions | `<header> <nav> <main> <footer>` | Only `<div>`s |
   | Lists | `<ul>/<ol>` | Line breaks |

2. **Language.** `<html lang="en">` (or the active language).

3. **Keyboard navigation.**
   - Every interactive element reachable with `Tab`, operable with `Enter`/`Space`, and in a logical order.
   - No keyboard traps; `Esc` closes dialogs and menus.
   - Add a **skip link** to `#main`.

4. **Focus management.**
   - Visible focus indicators everywhere (never `outline: none` without a replacement):
     ```css
     :focus-visible {
       outline: 3px solid var(--color-primary);
       outline-offset: 2px;
     }
     ```
   - When a dialog opens, move focus into it; when it closes, return focus to the trigger.
   - WCAG 2.2: focused elements must not be hidden behind sticky headers or cookie banners (use `scroll-padding-top`).

5. **Accessible forms.**

   ```tsx
   <label htmlFor="email">Email address</label>
   <input
     id="email"
     name="email"
     type="email"
     autoComplete="email"
     required
     aria-invalid={!!errors.email}
     aria-describedby={errors.email ? 'email-error' : undefined}
   />
   {errors.email && <p id="email-error" role="alert">{errors.email.message}</p>}
   ```

   - Visible labels (placeholders are not labels).
   - Errors in text, not colour alone, and explain how to fix them.
   - Use `autoComplete` attributes; WCAG 2.2 discourages making users re-enter information they already gave.

6. **Images and media.**
   - Meaningful images: descriptive `alt`. Decorative images: `alt=""`.
   - Videos: captions; audio: transcripts. Don't autoplay media with sound.

7. **Colour and contrast.** 4.5:1 for body text, 3:1 for large text, icons, and form borders. Never use colour alone to convey meaning.

8. **ARIA only when necessary.** "No ARIA is better than bad ARIA." Use it for things HTML can't express: `aria-expanded` on menu toggles, `aria-current="page"` on active nav links, `aria-live` regions for toast notifications.

9. **WCAG 2.2 additions to remember:** target size at least 24×24 CSS px; alternatives to dragging; accessible authentication (no puzzles or memory tests without an alternative); consistent help location.

10. **Motion.** Respect reduced motion:

    ```css
    @media (prefers-reduced-motion: reduce) {
      *,
      *::before,
      *::after {
        animation-duration: 0.01ms !important;
        transition-duration: 0.01ms !important;
        scroll-behavior: auto !important;
      }
    }
    ```

11. **Testing — automated [Automate]:**

    ```bash
    pnpm add -D @axe-core/playwright eslint-plugin-jsx-a11y
    ```

    ```ts
    // e2e/a11y.spec.ts
    import { test, expect } from "@playwright/test";
    import AxeBuilder from "@axe-core/playwright";

    for (const path of ["/", "/about", "/blog", "/contact"]) {
      test(`no a11y violations on ${path}`, async ({ page }) => {
        await page.goto(path);
        const results = await new AxeBuilder({ page })
          .withTags(["wcag2a", "wcag2aa", "wcag21aa", "wcag22aa"])
          .analyze();
        expect(results.violations).toEqual([]);
      });
    }
    ```

    Automated tools catch only part of the issues, so manual testing is required.

12. **Testing — manual [Manual]:**
    - Keyboard-only walkthrough of every page and form.
    - Screen reader: NVDA (Windows), VoiceOver (macOS/iOS), TalkBack (Android).
    - Zoom to 200% and 400% (content reflows at 320 px width without horizontal scrolling).
    - Check headings and landmarks with a browser extension (e.g. axe DevTools, WAVE, Accessibility Insights).

### Docs

- WCAG 2.2 Quick Reference: https://www.w3.org/WAI/WCAG22/quickref/
- WAI-ARIA Authoring Practices: https://www.w3.org/WAI/ARIA/apg/
- MDN Accessibility: https://developer.mozilla.org/en-US/docs/Web/Accessibility

### Common Mistakes

| Mistake                                | How to avoid it                              |
| -------------------------------------- | -------------------------------------------- |
| Removing focus outlines                | Style `:focus-visible` instead               |
| Clickable `div`s                       | Use buttons and links                        |
| Placeholder as label                   | Always use visible labels                    |
| Relying only on automated tools        | Do keyboard and screen-reader checks         |
| Over-using ARIA                        | Native HTML first                            |
| Cookie banner trapping or hiding focus | Test banners with keyboard and screen reader |

### Checklist

- [ ] Semantic landmarks and one `<h1>` per page
- [ ] Skip link works
- [ ] Every interactive element keyboard-accessible with visible focus
- [ ] Dialogs and menus manage focus and close with `Esc`
- [ ] Forms have labels, linked error messages, and autocomplete
- [ ] All images have appropriate alt text
- [ ] Contrast passes AA
- [ ] Reduced motion respected
- [ ] axe tests pass in CI
- [ ] Manual keyboard + screen-reader test done on key pages

---

## Phase 10: Performance Optimization

### Objectives and Outcomes

- Fast load and smooth interaction on real mobile devices and networks.
- **Outcome:** Core Web Vitals in the "Good" range and a lean, measured bundle.

### Targets (Core Web Vitals, 75th percentile of real users)

| Metric                              | Measures                           | Good     |
| ----------------------------------- | ---------------------------------- | -------- |
| **LCP** (Largest Contentful Paint)  | Loading speed of main content      | ≤ 2.5 s  |
| **INP** (Interaction to Next Paint) | Responsiveness to clicks/taps/keys | ≤ 200 ms |
| **CLS** (Cumulative Layout Shift)   | Visual stability                   | ≤ 0.1    |

### Step-by-Step

1. **Choose the right rendering per page.**
   | Strategy | Use for |
   | --- | --- |
   | Static generation (prerender at build) | Marketing pages, blog posts |
   | Static + on-demand revalidation (`revalidatePath`/`revalidateTag` from Payload hooks) | CMS content that changes occasionally |
   | Time-based revalidation | Content updated on a schedule |
   | Dynamic rendering per request | Personalised or user-specific pages |
   | Streaming with Suspense | Pages with a slow section; show the rest first |
   Pre-build dynamic routes with `generateStaticParams`:

   ```ts
   export async function generateStaticParams() {
     const posts = await getAllPublishedPosts();
     return posts.map((p) => ({ slug: p.slug }));
   }
   ```

   Check the caching model for your Next.js version in the docs, since it has changed between major versions.

2. **Images** with `next/image`:

   ```tsx
   <Image
     src={post.coverImage.url}
     alt={post.coverImage.alt}
     width={1200}
     height={630}
     sizes="(max-width: 768px) 100vw, 768px"
     priority
   />
   ```

   - Always set `width`/`height` (or `fill` with a sized parent) to prevent CLS.
   - Mark only the LCP image as `priority`; others lazy-load automatically.
   - Provide accurate `sizes`. Configure `remotePatterns` for CMS/CDN image domains.

3. **Fonts** with `next/font` (self-hosted, no layout shift):

   ```ts
   import { Inter } from "next/font/google";
   const inter = Inter({
     subsets: ["latin"],
     display: "swap",
     variable: "--font-sans",
   });
   ```

   Load only the weights and subsets you use.

4. **JavaScript.**
   - Keep most components as Server Components.
   - Lazy-load heavy client-only components:
     ```ts
     const Map = dynamic(() => import('@/components/Map'), { ssr: false, loading: () => <p>Loading map…</p> })
     ```
   - Load third-party scripts with `next/script` and `strategy="lazyOnload"` or `"afterInteractive"`. Audit every analytics/chat/tag script.
   - Avoid importing whole libraries (`import debounce from 'lodash/debounce'`, not all of lodash).

5. **Bundle analysis:**

   ```bash
   pnpm add -D @next/bundle-analyzer
   ANALYZE=true pnpm build
   ```

   ```ts
   // next.config.ts
   import bundleAnalyzer from "@next/bundle-analyzer";
   const withBundleAnalyzer = bundleAnalyzer({
     enabled: process.env.ANALYZE === "true",
   });
   export default withBundleAnalyzer(nextConfig);
   ```

6. **Caching and CDN.** Static assets get long-lived immutable caching automatically. Serve media from a CDN (most hosts and S3/R2 + CDN do this). Use `Cache-Control` headers deliberately on API routes.

7. **Data fetching.** Avoid waterfalls (fetch in parallel with `Promise.all`), limit Payload `depth`, use `select` to return only needed fields, paginate lists.

8. **Audits.**
   - Lab: Lighthouse in Chrome DevTools, PageSpeed Insights, WebPageTest. Always test production builds (`pnpm build && pnpm start`), never `next dev`.
   - Field (real users): PageSpeed Insights CrUX data, Search Console Core Web Vitals report, Vercel Speed Insights or `web-vitals` library.
   - [Automate] Lighthouse CI with budgets (Phase 14).

### Docs

- Web Vitals: https://web.dev/articles/vitals
- Next.js optimisation: https://nextjs.org/docs/app/getting-started/images and https://nextjs.org/docs/app/getting-started/fonts
- Lighthouse CI: https://github.com/GoogleChrome/lighthouse-ci

### Common Mistakes

| Mistake                              | How to avoid it                     |
| ------------------------------------ | ----------------------------------- |
| Measuring in dev mode                | Test production builds              |
| Images without dimensions            | Always set width/height or `fill`   |
| `priority` on many images            | Only the LCP image                  |
| Many third-party scripts in `<head>` | Load them late; remove unused ones  |
| Everything dynamic                   | Prerender and revalidate on publish |
| Uploading 5 MB images to the CMS     | Payload image sizes + `next/image`  |

### Checklist

- [ ] Rendering strategy chosen per route
- [ ] All images use `next/image` with correct sizes; LCP image prioritised
- [ ] Fonts via `next/font`, limited weights
- [ ] Heavy components lazy-loaded; third-party scripts audited
- [ ] Bundle analysed; no unexpected large dependencies
- [ ] Lighthouse mobile performance ≥ 90 on key pages (lab)
- [ ] LCP ≤ 2.5 s, INP ≤ 200 ms, CLS ≤ 0.1 targets set and monitored

---

## Phase 11: Security

### Objectives and Outcomes

- Protect users, data, and the server against common attacks (OWASP Top 10).
- **Outcome:** HTTPS, security headers, validated input, secure auth, rate limiting, safe secrets, and scanned dependencies.

### Step-by-Step

1. **HTTPS everywhere.** Hosts issue certificates automatically. Redirect HTTP → HTTPS and add HSTS once HTTPS works on all subdomains.

2. **Security headers** in `next.config.ts`:

   ```ts
   const securityHeaders = [
     {
       key: "Strict-Transport-Security",
       value: "max-age=63072000; includeSubDomains; preload",
     },
     { key: "X-Content-Type-Options", value: "nosniff" },
     { key: "Referrer-Policy", value: "strict-origin-when-cross-origin" },
     { key: "X-Frame-Options", value: "SAMEORIGIN" },
     {
       key: "Permissions-Policy",
       value: "camera=(), microphone=(), geolocation=()",
     },
   ];
   const nextConfig = {
     poweredByHeader: false,
     async headers() {
       return [{ source: "/(.*)", headers: securityHeaders }];
     },
   };
   export default nextConfig;
   ```

   - **Content Security Policy (CSP):** start in `Content-Security-Policy-Report-Only` mode, fix violations, then enforce. Next.js supports nonce-based CSP; follow the official CSP guide. Test that the Payload admin still works.
   - Only add `preload` to HSTS when you are sure every subdomain supports HTTPS.
   - Check results at https://securityheaders.com.

3. **Input validation and output encoding.** Validate all input with schemas (Phase 5). React escapes output by default.

4. **XSS prevention.**
   - Avoid `dangerouslySetInnerHTML`. If you must render HTML (e.g. from rich text), sanitise it (DOMPurify / `isomorphic-dompurify`) or render rich text through Payload's Lexical React renderer.
   - Never put user input into `href` without checking the protocol (block `javascript:` URLs).
   - Escape `<` in JSON-LD (see Phase 8).

5. **CSRF prevention.**
   - Use `SameSite=Lax` or `Strict` cookies.
   - Next.js Server Actions only accept POST and compare Origin/Host headers; configure `serverActions.allowedOrigins` if you use proxies.
   - Configure Payload's `csrf` and `cors` lists with your exact domains.
   - For custom cookie-authenticated endpoints, check the `Origin` header or use CSRF tokens.

6. **Authentication security.**
   - Strong password rules, login rate limiting/lockout (Payload has `maxLoginAttempts` and `lockTime`).
   - HTTP-only, Secure cookies; short session lifetimes for admins.
   - (Optional, recommended for admin users) Two-factor authentication.
   - Generic error messages ("Invalid email or password").

7. **Authorization.** Deny by default. Every Payload collection has explicit access rules; every custom endpoint checks the user's role. Test that an editor cannot do admin-only actions and anonymous users cannot read drafts.

8. **Rate limiting** on login, contact forms, search, and public APIs. Options: host/edge features (Vercel Firewall, Cloudflare), Upstash Ratelimit, `express-rate-limit` for Express. Add a CAPTCHA alternative (Cloudflare Turnstile, hCaptcha) to public forms if spam appears.

9. **Secret management.**
   - Secrets only in environment variables in the host's dashboard or a secret manager (Doppler, 1Password, AWS Secrets Manager).
   - Different secrets per environment; rotate on staff changes or leaks.
   - [Automate] Secret scanning: GitHub secret scanning / push protection, or `gitleaks` in CI.

10. **Dependency security [Automate].**

    ```bash
    pnpm audit --prod
    ```

    Enable Dependabot or Renovate for update PRs, and GitHub Dependabot alerts or Snyk. Keep Next.js and Payload on supported versions and apply security releases promptly.

11. **Secure API design.** Least data returned (no password hashes, internal fields); pagination limits; request body size limits; uploads restricted by MIME type and size; Payload field-level `access` to hide sensitive fields.

12. **File uploads.** Restrict MIME types and file size; never execute uploaded files; serve them from storage/CDN, not the app server.

13. **Privacy and compliance.** Privacy policy, cookie consent where required (GDPR, India's DPDP Act), collect only needed data, define data retention.

### Docs

- OWASP Top 10: https://owasp.org/www-project-top-ten/
- OWASP Cheat Sheets: https://cheatsheetseries.owasp.org/
- Next.js CSP: https://nextjs.org/docs/app/guides/content-security-policy
- Next.js security for Server Actions: https://nextjs.org/docs/app/guides/data-security
- Payload CORS/CSRF: https://payloadcms.com/docs/production/preventing-abuse

### Common Mistakes

| Mistake                                  | How to avoid it                       |
| ---------------------------------------- | ------------------------------------- |
| Secrets in Git or in `NEXT_PUBLIC_` vars | Secret scanning + env var review      |
| Open Payload access rules                | Explicit access functions, tested     |
| No rate limiting on forms and login      | Add limits before launch              |
| Rendering unsanitised HTML               | Sanitise or use the official renderer |
| Never updating dependencies              | Renovate/Dependabot + monthly updates |
| Enforcing a strict CSP without testing   | Report-only first                     |

### Checklist

- [ ] HTTPS enforced; HSTS on
- [ ] Security headers set; CSP tested (report-only → enforced)
- [ ] All inputs validated server-side
- [ ] No unsanitised HTML rendering
- [ ] CSRF/CORS configured for exact domains
- [ ] Login lockout, secure cookies; admin 2FA considered
- [ ] Access control tested for each role
- [ ] Rate limiting on login, forms, and public APIs
- [ ] Secrets only in env/secret manager; secret scanning on
- [ ] `pnpm audit` clean (or risks documented); Dependabot/Renovate on
- [ ] Privacy policy and cookie consent in place where required

---

## Phase 12: Testing and Quality Assurance

### Objectives and Outcomes

- Catch bugs before users do, and make it safe to change code later.
- **Outcome:** automated unit, integration, end-to-end, accessibility, and SEO checks in CI, plus a manual QA pass on staging.

### The Testing Pyramid

| Level                | What it tests                                                         | Tool                                     | How many                |
| -------------------- | --------------------------------------------------------------------- | ---------------------------------------- | ----------------------- |
| **Unit**             | Pure functions, utilities, small components                           | Vitest (or Jest) + React Testing Library | Many, fast              |
| **Integration**      | API routes, Server Actions, Payload hooks/access rules with a test DB | Vitest + Payload Local API               | Some                    |
| **End-to-end (E2E)** | Real user flows in a real browser                                     | Playwright                               | Few, for critical paths |

### Step-by-Step

1. **Unit tests:**

   ```bash
   pnpm add -D vitest @vitejs/plugin-react jsdom @testing-library/react @testing-library/jest-dom
   ```

   ```ts
   // src/lib/slugify.test.ts
   import { describe, it, expect } from "vitest";
   import { slugify } from "./slugify";
   describe("slugify", () => {
     it("creates lowercase hyphenated slugs", () => {
       expect(slugify("Hello World!")).toBe("hello-world");
     });
   });
   ```

   Note: async Server Components are best covered by E2E tests rather than unit tests.

2. **Integration tests:** test API routes and Payload access rules against a dedicated test database (never production). Example checks: anonymous user cannot read drafts; contact endpoint rejects invalid input with 400.

3. **E2E tests** for critical flows: navigation, contact form submit, search/filter, login to admin, 404 page.

   ```bash
   pnpm create playwright
   ```

   ```ts
   // e2e/contact.spec.ts
   import { test, expect } from "@playwright/test";
   test("visitor can send the contact form", async ({ page }) => {
     await page.goto("/contact");
     await page.getByLabel("Name").fill("Asha");
     await page.getByLabel("Email address").fill("asha@example.com");
     await page
       .getByLabel("Message")
       .fill("I would like a quote for a new website.");
     await page.getByRole("button", { name: "Send" }).click();
     await expect(page.getByRole("status")).toContainText("Thank you");
   });
   ```

   Use role and label selectors (they also check accessibility). Run E2E against a production build.

4. **Cross-browser testing:** Playwright projects for Chromium, Firefox, WebKit [Automate]; manual checks on real Safari iOS and Chrome Android [Manual]. (Optional) BrowserStack or LambdaTest for wider device coverage.

5. **Responsive testing:** Playwright mobile viewports [Automate] + manual check at 320, 375, 768, 1024, 1440 px [Manual].

6. **Accessibility testing:** axe in Playwright [Automate] + keyboard/screen reader pass [Manual] (Phase 9).

7. **SEO validation:**
   - [Automate] Test that key pages return 200, have a `<title>`, meta description, canonical, one `<h1>`; that `sitemap.xml` and `robots.txt` respond correctly.
   - [Manual] Rich Results Test, social share preview (LinkedIn Post Inspector, Facebook Sharing Debugger).
   - Broken links: `linkinator` or `lychee` [Automate].

8. **Performance:** Lighthouse CI on key URLs [Automate].

9. **Production build verification [Automate]:**

   ```bash
   pnpm build && pnpm start
   ```

   The build must pass with no type errors and no warnings you haven't reviewed.

10. **Manual QA on staging [Manual]:** use a test plan based on the user stories' acceptance criteria. Log bugs with steps to reproduce, expected vs actual result, screenshots, browser/device.

11. **User acceptance testing (UAT):** the client tests on staging and signs off.

### Docs

- Next.js testing: https://nextjs.org/docs/app/guides/testing
- Vitest: https://vitest.dev
- Testing Library: https://testing-library.com
- Playwright: https://playwright.dev

### Common Mistakes

| Mistake                                    | How to avoid it                              |
| ------------------------------------------ | -------------------------------------------- |
| Only manual testing                        | Automate critical flows and run them in CI   |
| Brittle selectors (CSS classes, nth-child) | Use roles, labels, and text                  |
| Tests hitting the production database      | Dedicated test/staging DB                    |
| Testing only on Chrome desktop             | Add Firefox, WebKit, and real mobile devices |
| Skipping UAT                               | Get written client sign-off on staging       |

### Checklist

- [ ] Unit tests for utilities and key components
- [ ] Integration tests for APIs and access rules
- [ ] E2E tests for all critical flows on Chromium, Firefox, WebKit
- [ ] Responsive checks done
- [ ] Accessibility tests automated and manual
- [ ] SEO checks and broken-link scan pass
- [ ] Production build passes
- [ ] Manual QA test plan executed; critical bugs fixed
- [ ] Client UAT sign-off

---

## Phase 13: Analytics and Monitoring

### Objectives and Outcomes

- Know how the site is used, whether it is up, how fast it is, and when it breaks.
- **Outcome:** analytics, Search Console, error tracking, uptime alerts, performance monitoring, and structured logs.

### Step-by-Step

1. **Analytics — choose one:**
   | Option | Notes |
   | --- | --- |
   | Google Analytics 4 | Free, powerful; needs a cookie consent banner in many regions |
   | Plausible / Umami / Fathom | Privacy-friendly, lightweight, often cookieless; Umami can be self-hosted |
   | Vercel Web Analytics | Simple if hosted on Vercel |

   ```tsx
   // GA4 via the official Next.js helper
   import { GoogleAnalytics } from "@next/third-parties/google";
   // in the root layout body:
   <GoogleAnalytics gaId={process.env.NEXT_PUBLIC_GA_ID!} />;
   ```

   - Load analytics only in production and only after consent where required.
   - Define key events/conversions (form submit, CTA click, phone click) during Phase 1, track them here.

2. **Google Search Console:** verify the domain (DNS TXT record is best, see Phase 16), submit the sitemap, monitor indexing, Core Web Vitals, and manual actions. (Optional) Bing Webmaster Tools.

3. **Error tracking:** Sentry (has a Next.js SDK covering client, server, and edge):

   ```bash
   npx @sentry/wizard@latest -i nextjs
   ```

   Set environment tags (`development`, `staging`, `production`), upload source maps in CI, filter out noise, and scrub personal data.

4. **Uptime monitoring:** UptimeRobot, Better Stack, or Checkly. Monitor the home page, a key content page, and a health endpoint every 1–5 minutes; alert by email/Slack/SMS.

   ```ts
   // src/app/api/health/route.ts
   export const dynamic = "force-dynamic";
   export async function GET() {
     // Optional: check the database connection here
     return Response.json({ ok: true, time: new Date().toISOString() });
   }
   ```

5. **Performance monitoring (real users):** Vercel Speed Insights, Sentry performance, or report `web-vitals` to your analytics. Watch Search Console's Core Web Vitals report.

6. **Logging:**
   - Use structured JSON logs (e.g. `pino`) with a level, message, and request ID.
   - Never log passwords, tokens, or full personal data.
   - Send logs to your host's log viewer or a log service (Better Stack, Axiom, Datadog).

7. **Alerts and dashboards.** Decide who gets alerted, for what, and how fast they must respond. Avoid alert fatigue: alert on user-impacting problems only.

### Docs

- Search Console: https://search.google.com/search-console/about
- Sentry for Next.js: https://docs.sentry.io/platforms/javascript/guides/nextjs/
- Next.js third-party libraries: https://nextjs.org/docs/app/guides/third-party-libraries

### Common Mistakes

| Mistake                                       | How to avoid it                            |
| --------------------------------------------- | ------------------------------------------ |
| Analytics firing in dev/staging               | Load only when `production`                |
| No consent handling                           | Use a consent tool or cookieless analytics |
| Errors only noticed when the client complains | Sentry + uptime alerts before launch       |
| Logging secrets or personal data              | Scrub logs; review what is logged          |
| No defined conversions                        | Agree key events in planning               |

### Checklist

- [ ] Analytics installed, production-only, consent-compliant
- [ ] Key events/conversions tracked
- [ ] Search Console verified (sitemap submitted at launch)
- [ ] Sentry capturing client and server errors with source maps
- [ ] Uptime monitors and alerts configured
- [ ] Real-user performance monitoring on
- [ ] Structured logs without sensitive data

---

## Phase 14: CI/CD and DevOps

### Objectives and Outcomes

- Every change is reviewed, checked automatically, and deployed the same way every time, with a way back.
- **Outcome:** a branching strategy, PR reviews, a CI pipeline (lint, types, tests, build), automated deployments, and a rollback plan.

### Step-by-Step

1. **Git branching strategy.** For most website projects, use **trunk-based development / GitHub Flow**:

   ```text
   main            → always deployable; deploys to production
   staging (Optional) → deploys to staging (or use preview deployments)
   feature/*       → short-lived branches, merged via pull request
   fix/*           → bug fixes
   ```

   ```bash
   git checkout -b feature/contact-form
   # work, commit small changes
   git commit -m "feat(contact): add server-side validation"
   git push -u origin feature/contact-form
   # open a pull request → review → merge to main
   ```

   - Use **Conventional Commits** (`feat:`, `fix:`, `chore:`, `docs:`, `refactor:`, `test:`).
   - **Branch protection on `main`:** require PRs, at least one approval, passing CI, and up-to-date branches; block force pushes.
   - (Optional) GitFlow (`develop`, `release/*`) only if you ship scheduled releases with multiple versions.

2. **Code reviews.** Use a PR template:

   ```markdown
   ## What changed

   ## Why

   ## How to test

   ## Screenshots (UI changes)

   ## Checklist

   - [ ] Tests added/updated
   - [ ] Accessible (keyboard, labels, contrast)
   - [ ] No secrets or console logs
   - [ ] Migrations included if the schema changed
   ```

3. **CI pipeline (GitHub Actions):**

   ```yaml
   # .github/workflows/ci.yml
   name: CI
   on:
     pull_request:
     push:
       branches: [main]

   jobs:
     quality:
       runs-on: ubuntu-latest
       env:
         PAYLOAD_SECRET: ci-secret-not-real
         DATABASE_URI: ${{ secrets.CI_DATABASE_URI }}
         NEXT_PUBLIC_SITE_URL: http://localhost:3000
       steps:
         - uses: actions/checkout@v4
         - uses: pnpm/action-setup@v4
         - uses: actions/setup-node@v4
           with:
             node-version-file: ".nvmrc"
             cache: "pnpm"
         - run: pnpm install --frozen-lockfile
         - run: pnpm format:check
         - run: pnpm lint
         - run: pnpm typecheck
         - run: pnpm test
         - run: pnpm build

     e2e:
       needs: quality
       runs-on: ubuntu-latest
       env:
         PAYLOAD_SECRET: ci-secret-not-real
         DATABASE_URI: ${{ secrets.CI_DATABASE_URI }}
         NEXT_PUBLIC_SITE_URL: http://localhost:3000
       steps:
         - uses: actions/checkout@v4
         - uses: pnpm/action-setup@v4
         - uses: actions/setup-node@v4
           with: { node-version-file: ".nvmrc", cache: "pnpm" }
         - run: pnpm install --frozen-lockfile
         - run: pnpm exec playwright install --with-deps
         - run: pnpm build
         - run: pnpm test:e2e
         - uses: actions/upload-artifact@v4
           if: failure()
           with: { name: playwright-report, path: playwright-report }
   ```

   (Optional) Use a service container (`services: postgres:` or `mongo:`) instead of a hosted CI database. Pin action versions to the latest majors when you set this up.

4. **Lighthouse CI (Optional, recommended):**

   ```json
   // lighthouserc.json
   {
     "ci": {
       "collect": {
         "startServerCommand": "pnpm start",
         "url": ["http://localhost:3000/", "http://localhost:3000/blog"]
       },
       "assert": {
         "assertions": {
           "categories:performance": ["warn", { "minScore": 0.9 }],
           "categories:accessibility": ["error", { "minScore": 0.95 }],
           "categories:seo": ["error", { "minScore": 0.95 }]
         }
       }
     }
   }
   ```

5. **Deployment pipeline.**
   - **Git-integrated hosts (Vercel, Netlify, Railway, Render):** connect the repo; PRs get preview deployments, `main` deploys to production. Make the host wait for CI ("required checks") or deploy only after CI passes.
   - **Custom pipelines (VPS, Docker, AWS):** add a deploy job that runs only on `main` after CI passes, e.g. build a Docker image, push to a registry, and roll it out.
   - **Database migrations:** run `pnpm payload migrate` as a step before the new version serves traffic (e.g. in the build command or a release step). Write migrations to be backward compatible so a rollback of the app still works.
   - Use GitHub **Environments** (staging, production) with their own secrets and optional manual approval for production.

6. **GitLab CI/CD (if you use GitLab):** the same stages (`install → lint → typecheck → test → build → deploy`) in `.gitlab-ci.yml`, with protected branches and environment-scoped variables.

7. **Rollback strategy.**
   - **Instant rollback:** Vercel/Netlify/Render let you promote a previous deployment in one click. Know where this button is before launch.
   - **Git revert:** `git revert <commit>` then push to `main` to redeploy a fixed version.
   - **Docker:** redeploy the previous image tag (tag images with the Git SHA).
   - **Database:** prefer forward-fixing with a new migration; restore from backup only as a last resort (data written since the backup is lost).
   - **(Optional) Feature flags** to switch off a broken feature without redeploying.

### Docs

- GitHub Actions: https://docs.github.com/actions
- GitLab CI/CD: https://docs.gitlab.com/ci/
- Conventional Commits: https://www.conventionalcommits.org

### Common Mistakes

| Mistake                                | How to avoid it                |
| -------------------------------------- | ------------------------------ |
| Pushing directly to `main`             | Branch protection              |
| CI not blocking deployment             | Make CI a required check       |
| Migrations run manually (or forgotten) | Automate in the release step   |
| No tested rollback                     | Practise a rollback on staging |
| Long-lived feature branches            | Merge small PRs often          |

### Checklist

- [ ] Branching strategy and commit convention documented
- [ ] Branch protection and required reviews on `main`
- [ ] PR template in place
- [ ] CI runs format, lint, typecheck, tests, build, E2E
- [ ] Deployments automatic after CI passes; previews for PRs
- [ ] Migrations run automatically in the pipeline
- [ ] Separate secrets for staging and production
- [ ] Rollback procedure documented and tested

---

## Phase 15: Deployment and Hosting

### Objectives and Outcomes

- The app runs reliably on suitable hosting, with separate staging and production environments.
- **Outcome:** hosting chosen, production database and storage configured, env vars set, staging and production deployed.

### Step-by-Step

1. **Choose hosting.**
   | Option | Best for | Notes for Payload |
   | --- | --- | --- |
   | **Vercel** | Next.js + Payload with a managed DB | Serverless: use a hosted DB (Neon/Supabase/Atlas) and a storage adapter (Vercel Blob/S3). Watch function time limits. |
   | **Netlify** | Similar to Vercel | Same serverless considerations |
   | **Railway / Render / Fly.io** | Long-running Node server, simple setup | Persistent server; can host DB too; good for Express services |
   | **VPS (DigitalOcean, Hetzner, AWS Lightsail) + Docker** | Full control, predictable cost | You manage OS updates, reverse proxy (Nginx/Caddy), SSL, backups |
   | **AWS / GCP / Azure** (Optional) | Enterprise, compliance, scale | More setup; ECS/App Runner/Cloud Run for containers |
   **Express service:** host on Railway, Render, Fly.io, or a VPS (not serverless if it needs WebSockets or long jobs).

2. **Production database.** Managed provider in the region closest to your app server (e.g. Mumbai for Indian users). Enable backups, TLS, and restricted network access (Phase 7).

3. **Media storage.** Configure a Payload storage adapter for production:

   ```ts
   import { s3Storage } from "@payloadcms/storage-s3";
   // in buildConfig plugins:
   plugins: [
     s3Storage({
       collections: { media: true },
       bucket: process.env.S3_BUCKET!,
       config: {
         region: process.env.S3_REGION,
         credentials: {
           accessKeyId: process.env.S3_ACCESS_KEY_ID!,
           secretAccessKey: process.env.S3_SECRET_ACCESS_KEY!,
         },
       },
     }),
   ];
   ```

4. **Environment variables per environment.** Set them in the host dashboard:
   | Variable | Development | Staging | Production |
   | --- | --- | --- | --- |
   | `NEXT_PUBLIC_SITE_URL` | `http://localhost:3000` | `https://staging.clientsite.com` | `https://clientsite.com` |
   | `DATABASE_URI` | local DB | staging DB | production DB |
   | `PAYLOAD_SECRET` | dev value | unique | unique, strong |
   | `S3_BUCKET` | (local disk OK) | staging bucket | production bucket |
   | `SENTRY_DSN` | empty | set (env=staging) | set (env=production) |
   | `NEXT_PUBLIC_GA_ID` | empty | empty | set |

5. **Build settings.**
   - Install: `pnpm install --frozen-lockfile`
   - Build: `pnpm payload migrate && pnpm build` (or run migrations in a separate release step)
   - Start (non-serverless): `pnpm start`
   - Node version matches `.nvmrc`.
   - (Optional, Docker/VPS) set `output: 'standalone'` in `next.config.ts` for a smaller image:

     ```dockerfile
     FROM node:22-alpine AS deps
     WORKDIR /app
     RUN corepack enable
     COPY package.json pnpm-lock.yaml ./
     RUN pnpm install --frozen-lockfile

     FROM node:22-alpine AS build
     WORKDIR /app
     RUN corepack enable
     COPY --from=deps /app/node_modules ./node_modules
     COPY . .
     RUN pnpm build

     FROM node:22-alpine AS run
     WORKDIR /app
     ENV NODE_ENV=production
     COPY --from=build /app/.next/standalone ./
     COPY --from=build /app/.next/static ./.next/static
     COPY --from=build /app/public ./public
     USER node
     EXPOSE 3000
     CMD ["node", "server.js"]
     ```

6. **Deploy staging first.** Deploy, seed or copy anonymised content, run smoke tests and UAT there. Protect staging with a password or IP restriction and `noindex`.

7. **Deploy production.** Same build, production env vars. Initially test on the host's default URL (e.g. `my-site.vercel.app`) before connecting the real domain.

8. **Create the first admin user** in production with a strong password, then invite editors with the `editor` role.

### Docs

- Next.js deploying: https://nextjs.org/docs/app/getting-started/deploying
- Payload production deployment: https://payloadcms.com/docs/production/deployment
- Payload storage adapters: https://payloadcms.com/docs/upload/storage-adapters

### Common Mistakes

| Mistake                                                           | How to avoid it                         |
| ----------------------------------------------------------------- | --------------------------------------- |
| Media saved to local disk on serverless hosting (files disappear) | Use a storage adapter                   |
| Database far from the app server                                  | Same region                             |
| Production env vars copied into staging                           | Unique values per environment           |
| Deploying straight to production                                  | Staging first, every time               |
| Default admin credentials                                         | Strong unique password, 2FA if possible |

### Checklist

- [ ] Hosting chosen with reasons documented
- [ ] Production DB in the right region with backups
- [ ] Storage adapter working for media
- [ ] Env vars set for staging and production; none missing
- [ ] Migrations run during deployment
- [ ] Staging deployed, password-protected, `noindex`
- [ ] Production deployed and working on the host URL
- [ ] Admin and editor accounts created securely

---

## Phase 16: Domain and DNS Configuration

### Objectives and Outcomes

- The client's domain points to the production site over HTTPS, with email and verification records intact.
- **Outcome:** domain registered and owned by the client, DNS records configured, SSL active, HTTPS and canonical host enforced.

### Step-by-Step

1. **Choose a domain name.** Short, easy to spell and say, no hyphens or numbers if possible, matches the brand. Choose the right TLD (`.com`, `.in`, `.co.in`, `.org`, etc.). Check trademarks and social handles.

2. **Register the domain** at a reputable registrar (Cloudflare Registrar, Namecheap, Porkbun, GoDaddy, Squarespace Domains, or a local `.in` registrar).
   - **Register in the client's name and account**, or transfer ownership at handover.
   - Enable auto-renew, WHOIS privacy, registrar lock, and 2FA on the registrar account.

3. **Understand DNS records:**
   | Record | Purpose | Example |
   | --- | --- | --- |
   | `A` | Domain → IPv4 address | `@ → 76.76.21.21` (value from your host) |
   | `AAAA` | Domain → IPv6 address | `@ → 2606:…` |
   | `CNAME` | Alias to another hostname (not on the root/apex) | `www → cname.vercel-dns.com` |
   | `MX` | Where email is delivered | Google Workspace / Microsoft 365 / Zoho values |
   | `TXT` | Verification and email security | Search Console verification, SPF, DMARC |
   | `CAA` (Optional) | Which certificate authorities may issue SSL | `0 issue "letsencrypt.org"` |
   | `NS` | Which DNS provider is authoritative | Registrar's or Cloudflare's nameservers |

4. **Decide where DNS is managed:** at the registrar, at Cloudflare (free, fast, extra security features), or at the host (e.g. Vercel nameservers). Pick one and document it.

5. **Connect the domain to the host.**
   1. Add `clientsite.com` and `www.clientsite.com` in the host's domain settings.
   2. Add the exact records the host shows you (usually an `A` record for the apex and a `CNAME` for `www`).
   3. Choose the **canonical host** (e.g. `clientsite.com`) and set the other to redirect permanently.
   4. Wait for propagation (minutes to a few hours; up to 48 h in rare cases). Check with:
      ```bash
      dig clientsite.com A +short
      dig www.clientsite.com CNAME +short
      nslookup clientsite.com
      ```
      Or use https://dnschecker.org.
   - **Tip:** lower the TTL (e.g. to 300 seconds) a day before switching an existing domain so changes take effect faster.

6. **Subdomains (when needed):**
   - `staging.clientsite.com` → staging deployment
   - `cms.clientsite.com` (Optional) if the admin is hosted separately
   - `api.clientsite.com` (Optional) for a separate Express service

7. **SSL/TLS certificates.** Most hosts issue and renew Let's Encrypt certificates automatically after DNS points to them. On a VPS use Caddy (automatic) or Certbot with Nginx. If you use Cloudflare's proxy, set SSL mode to **Full (strict)**.

8. **Enforce HTTPS.** HTTP → HTTPS redirect (host setting), HSTS header (Phase 11), update `NEXT_PUBLIC_SITE_URL` and Payload `serverURL`, CORS, and CSRF lists to the final `https://` domain.

9. **Email records (if the domain sends or receives email).** Do not delete existing `MX` records when changing DNS. Add SPF, DKIM, and DMARC so form notification emails are not marked as spam:

   ```text
   TXT  @       "v=spf1 include:_spf.google.com include:amazonses.com ~all"
   TXT  _dmarc  "v=DMARC1; p=quarantine; rua=mailto:dmarc@clientsite.com"
   ```

   (Use the exact values from your email provider.)

10. **Domain verification.** Add TXT records for Google Search Console (domain property), email provider, and any other services.

11. **Migrating from an existing site:** export the current DNS zone first, recreate every record (especially `MX` and `TXT`), map old URLs to new ones with redirects (Phase 8), then switch.

### Docs

- Vercel domains: https://vercel.com/docs/domains
- Cloudflare DNS: https://developers.cloudflare.com/dns/
- Let's Encrypt: https://letsencrypt.org/docs/

### Common Mistakes

| Mistake                                               | How to avoid it                                                   |
| ----------------------------------------------------- | ----------------------------------------------------------------- |
| Domain registered in the developer's personal account | Register in the client's account                                  |
| Deleting MX records and breaking company email        | Export the zone and keep email records                            |
| `CNAME` on the root domain where unsupported          | Use `A`/`ALIAS`/`ANAME` or CNAME flattening as the host instructs |
| Both `www` and non-`www` serving the site             | Redirect one to the canonical host                                |
| Cloudflare SSL set to "Flexible" (redirect loops)     | Use "Full (strict)"                                               |
| Forgetting auto-renew                                 | Turn on auto-renew and set a calendar reminder                    |

### Checklist

- [ ] Domain owned by the client with auto-renew, lock, and 2FA
- [ ] DNS provider decided and documented
- [ ] Apex and `www` connected; one redirects to the other
- [ ] SSL active; HTTPS enforced
- [ ] Site URL, Payload `serverURL`, CORS/CSRF updated to the final domain
- [ ] MX, SPF, DKIM, DMARC correct; test email delivered
- [ ] Verification TXT records added (Search Console, etc.)
- [ ] Staging subdomain set up (if used)

---

## Phase 17: Production Launch Checklist

### Objectives and Outcomes

- A calm, predictable launch with nothing forgotten.
- **Outcome:** site live on the final domain, verified, indexed, monitored, and backed up.

### Before Launch (1–3 days before)

**Content [Manual]**

- [ ] All placeholder text and "lorem ipsum" removed
- [ ] Spelling, grammar, names, phone numbers, addresses, prices checked
- [ ] Legal pages live: privacy policy, terms, cookie policy
- [ ] Images have alt text; no test or unlicensed images
- [ ] Favicon, app icons, and `manifest` set
- [ ] Custom 404 (and 500) pages styled

**SEO**

- [ ] Unique titles, descriptions, and canonicals [Automate + Manual spot check]
- [ ] Open Graph previews checked in share debuggers [Manual]
- [ ] `robots.txt` allows indexing in production and blocks admin [Manual]
- [ ] No `noindex` left on production pages [Automate]
- [ ] `sitemap.xml` correct [Automate]
- [ ] Redirects from old URLs tested [Automate]
- [ ] Broken-link scan clean [Automate]

**Technical**

- [ ] All CI checks pass on the release commit
- [ ] Production env vars reviewed; no dev/staging values
- [ ] Migrations applied in production
- [ ] Security headers verified (securityheaders.com) [Manual]
- [ ] Access control tested: anonymous users cannot see drafts or admin data [Automate + Manual]
- [ ] Forms deliver emails and store submissions [Manual]
- [ ] Rate limiting and spam protection active
- [ ] Lighthouse scores meet targets on mobile [Automate]
- [ ] Accessibility automated + manual checks done

**Operations**

- [ ] Database backup taken right before launch; restore tested earlier
- [ ] Media storage backed up/versioned
- [ ] Sentry, uptime monitors, and alerts active
- [ ] Analytics working in production only, with consent
- [ ] Rollback procedure documented; team knows how to use it
- [ ] Launch time chosen when traffic is low and the team is available (not Friday evening)

### Launch Day

1. Lower DNS TTL beforehand (if switching an existing domain).
2. Deploy the final release to production.
3. Point DNS / attach the domain (Phase 16).
4. Verify HTTPS and canonical redirect (`http://`, `www`, non-`www` all end at one `https://` URL).
5. **Smoke tests [Manual + Automate]:** run E2E smoke tests against the production URL, then manually check: home, key pages, navigation, mobile menu, contact form, search, 404, admin login, publishing a test change (then reverting it).
6. Google Search Console: verify property, **submit the sitemap**, URL Inspection → request indexing for the home page and key pages.
7. Check Sentry and server logs for new errors.
8. Announce launch to the client.

### After Launch (first week)

- [ ] Monitor errors, uptime, and Core Web Vitals daily
- [ ] Check Search Console coverage/indexing reports
- [ ] Confirm analytics data is arriving
- [ ] Raise DNS TTL back to normal
- [ ] Hand over: credentials (via a password manager), editor guide, README, architecture notes, maintenance plan
- [ ] Hold a short retrospective: what to improve next project

---

## Phase 18: Post-Launch Maintenance

### Objectives and Outcomes

- The site stays secure, fast, accurate, and recoverable over time.
- **Outcome:** a written maintenance schedule, incident response plan, and disaster recovery plan agreed with the client.

### Maintenance Schedule

| Frequency      | Task                                                                      | Type       |
| -------------- | ------------------------------------------------------------------------- | ---------- |
| **Continuous** | Uptime and error alerts                                                   | [Automate] |
| **Continuous** | Dependabot/Renovate PRs and security alerts                               | [Automate] |
| **Weekly**     | Review Sentry errors and uptime reports                                   | [Manual]   |
| **Weekly**     | Merge safe patch updates after CI passes                                  | [Manual]   |
| **Monthly**    | Minor dependency updates (Next.js, Payload, React) on staging first       | [Manual]   |
| **Monthly**    | Review Core Web Vitals, Search Console coverage, and analytics            | [Manual]   |
| **Monthly**    | Broken-link scan                                                          | [Automate] |
| **Quarterly**  | Test a backup restore                                                     | [Manual]   |
| **Quarterly**  | Accessibility spot check on new content and pages                         | [Manual]   |
| **Quarterly**  | Review user accounts and roles; remove ex-staff; rotate secrets if needed | [Manual]   |
| **Yearly**     | Major version upgrades (planned, tested on staging)                       | [Manual]   |
| **Yearly**     | Domain and SSL renewal check, legal pages review, full audit              | [Manual]   |

### Step-by-Step

1. **Dependency updates and security patches.**

   ```bash
   pnpm outdated
   pnpm update            # within allowed ranges
   pnpm audit --prod
   ```

   Apply critical security releases (especially Next.js, React, Payload) within days. Always deploy to staging and run the full test suite first. Read release notes and upgrade guides for major versions.

2. **Database and media backups.** Confirm automated backups run; test a restore quarterly.

3. **Monitoring.** Keep alerts routed to someone who will respond. Review trends, not just incidents.

4. **Performance improvements.** Re-run Lighthouse after big content or feature changes; check new third-party scripts; watch real-user Core Web Vitals.

5. **SEO tracking.** Monitor rankings, impressions, and clicks in Search Console; fix crawl errors; add redirects when content is moved or deleted; refresh outdated content.

6. **Content updates.** Editors handle routine changes. Developers handle new templates or blocks. Keep the editor guide current.

7. **Incident response.**
   1. **Detect:** alert or user report.
   2. **Assess:** what is broken, how many users, is data at risk?
   3. **Communicate:** tell the client early; status updates at set intervals.
   4. **Mitigate:** roll back to the last good deployment or disable the feature.
   5. **Fix:** proper fix through the normal PR + CI process.
   6. **Review:** short blameless post-mortem: cause, impact, timeline, and actions to prevent a repeat.
      For security incidents: rotate affected secrets, revoke sessions, check logs, and follow legal notification duties.

8. **Disaster recovery.** Document and test:
   - **RPO** (how much data you can afford to lose, e.g. 24 h → daily backups) and **RTO** (how fast you must recover, e.g. 4 h).
   - Steps to rebuild: redeploy code from Git, restore DB from backup, restore media, reconnect domain, re-set env vars from the secret manager.
   - Where credentials and runbooks live (shared password manager, not one person's laptop).

9. **Maintenance agreement.** Agree in writing with the client: what is included (updates, monitoring, backups, small content fixes), response times, and how new features are quoted.

### Common Mistakes

| Mistake                         | How to avoid it                        |
| ------------------------------- | -------------------------------------- |
| "Launch and forget"             | Agree a maintenance plan before launch |
| Updating directly in production | Staging first, then production         |
| Ignoring security advisories    | Automated alerts + fast patch policy   |
| Backups never restored          | Quarterly restore test                 |
| Knowledge in one person's head  | Runbooks and README in the repo        |

### Checklist

- [ ] Maintenance schedule agreed with the client
- [ ] Dependency and security update process running
- [ ] Backups verified with a recent restore test
- [ ] Incident response steps documented
- [ ] Disaster recovery plan with RPO/RTO documented and tested
- [ ] Credentials stored in a shared password manager

---

## Project Milestones

A realistic plan for a typical 8–15 page business website with a blog and CMS, built by one or two developers. Adjust for size, content readiness, and client response times.

| #   | Milestone              | Phases | Typical duration        | Deliverable / sign-off                                          |
| --- | ---------------------- | ------ | ----------------------- | --------------------------------------------------------------- |
| M1  | Discovery and scope    | 1      | 1 week                  | Signed scope, sitemap, timeline                                 |
| M2  | Design approved        | 2      | 1–3 weeks               | Approved mobile + desktop designs, design system                |
| M3  | Foundation ready       | 3, 7   | 2–4 days                | Repo, CI, Payload + DB running, staging deployed                |
| M4  | CMS and content model  | 6      | 3–5 days                | Collections, globals, roles; editors can start entering content |
| M5  | Frontend build         | 4, 5   | 2–4 weeks               | All templates built and connected to CMS on staging             |
| M6  | Quality pass           | 8–13   | 1–2 weeks (overlaps M5) | SEO, a11y, performance, security, tests, monitoring done        |
| M7  | UAT and content freeze | 12     | 1 week                  | Client sign-off on staging; final content in place              |
| M8  | Launch                 | 15–17  | 1–2 days                | Live on the final domain; launch checklist complete             |
| M9  | Hypercare              | 17–18  | 1–2 weeks               | Post-launch fixes, handover, maintenance plan active            |

**Total:** roughly 7–12 weeks. Content delays and slow feedback are the most common reasons this grows; set response-time expectations in the scope document.

---

## Automated vs Manual Tasks

| Area          | Automate (CI / tools / scripts)                                        | Requires manual verification                                        |
| ------------- | ---------------------------------------------------------------------- | ------------------------------------------------------------------- |
| Code quality  | Formatting, linting, type checking, build                              | Code review for logic, readability, architecture                    |
| Testing       | Unit, integration, E2E, cross-browser runs                             | Exploratory testing, UAT, real-device checks                        |
| Accessibility | axe checks, `jsx-a11y` lint rules                                      | Keyboard walkthrough, screen reader, zoom, content clarity          |
| SEO           | Meta tag presence, sitemap/robots checks, broken links, Lighthouse SEO | Content quality, keyword fit, share previews, Search Console review |
| Performance   | Lighthouse CI budgets, bundle size checks                              | Real-device feel, reviewing field data and trade-offs               |
| Security      | Dependency audit, secret scanning, Dependabot/Renovate                 | Access-control review, CSP tuning, threat review of new features    |
| Database      | Scheduled backups, migrations in the pipeline                          | Restore tests, migration review before production                   |
| Deployment    | Builds, previews, production deploys after CI                          | Production approval, smoke tests, rollback decisions                |
| Monitoring    | Uptime checks, error alerts, log collection                            | Triage, incident communication, post-mortems                        |
| Content       | Image resizing, required fields (e.g. alt text)                        | Accuracy, tone, legal review, alt text quality                      |

Rule of thumb: automate anything that is **repeatable and has a clear pass/fail**. Keep humans on anything that needs **judgement**.

---

## Final Production-Readiness Checklist

Use this as the final gate before launch. Every item should be ticked or explicitly accepted as a known risk by the client.

### Planning and Design

- [ ] Scope, sitemap, and acceptance criteria met
- [ ] Designs implemented at all breakpoints with all component states

### Code and Architecture

- [ ] TypeScript strict; no type errors; lint and format clean
- [ ] Express/extra services only where justified and documented
- [ ] README, `.env.example`, and architecture notes up to date

### CMS and Data

- [ ] Explicit access control on every collection and global; roles tested
- [ ] Drafts, preview, publish, and cache revalidation work
- [ ] Media on persistent storage; alt text required
- [ ] Migrations committed and applied; backups automatic; restore tested

### SEO

- [ ] Per-page titles, descriptions, canonicals, Open Graph
- [ ] Valid structured data
- [ ] Sitemap and robots.txt correct for production; staging not indexable
- [ ] Redirects in place; one canonical host

### Accessibility

- [ ] WCAG 2.2 AA: automated checks pass; manual keyboard and screen-reader checks done

### Performance

- [ ] Core Web Vitals targets met in lab; field monitoring on
- [ ] Images, fonts, and scripts optimised

### Security

- [ ] HTTPS + HSTS; security headers; CSP tested
- [ ] Server-side validation, rate limiting, spam protection
- [ ] Secrets safe; dependencies audited; admin accounts secured

### Testing

- [ ] Unit, integration, E2E tests pass in CI on all target browsers
- [ ] Manual QA and client UAT signed off

### Operations

- [ ] CI/CD with required checks; automated deploys; rollback tested
- [ ] Staging and production fully separated
- [ ] Analytics, Search Console, Sentry, uptime alerts, and logs active
- [ ] Domain owned by the client; DNS, SSL, and email records correct

### Handover

- [ ] Credentials shared via a password manager
- [ ] Editor guide delivered and tested by an editor
- [ ] Maintenance plan, incident response, and disaster recovery documented

---

## Key Documentation Links

| Topic                    | Link                                      |
| ------------------------ | ----------------------------------------- |
| Next.js                  | https://nextjs.org/docs                   |
| React                    | https://react.dev                         |
| TypeScript               | https://www.typescriptlang.org/docs/      |
| Payload CMS              | https://payloadcms.com/docs               |
| Express                  | https://expressjs.com                     |
| Node.js                  | https://nodejs.org/docs/latest/api/       |
| Google Search Central    | https://developers.google.com/search/docs |
| WCAG 2.2 Quick Reference | https://www.w3.org/WAI/WCAG22/quickref/   |
| web.dev (performance)    | https://web.dev                           |
| OWASP Cheat Sheets       | https://cheatsheetseries.owasp.org        |
| Playwright               | https://playwright.dev                    |
| Vitest                   | https://vitest.dev                        |
| GitHub Actions           | https://docs.github.com/actions           |
| MDN Web Docs             | https://developer.mozilla.org             |

> Tools and framework APIs change. Before each new project, check the official docs and release notes for Next.js and Payload, and update the commands and config examples in this file.

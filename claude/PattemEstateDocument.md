# Pattem Estates Payload: Project Document

## 1. Overview

Pattem Estates V2 is a real-estate website built as a single **Next.js 16 + Payload CMS 3** application (a monolith). The public site and the CMS admin panel run in the same app and share one MongoDB database.

- Public site: `http://localhost:3000`
- Admin panel: `http://localhost:3000/admin`
- Production: https://www.pattemestates.com, staging: https://staging.pattemestates.com

Content editors build pages in the admin from reusable blocks. Visitors browse properties, locations, builders and interiors, search, use calculators, and submit lead forms that feed a built-in CRM.

## 2. Main Features

- **Block-based pages**: pages are assembled from blocks in [src/blocks/](src/blocks/) (home, location, property details, builders, blogs, EMI calculator, forms, legal and more) and rendered by `RenderBlocks.tsx`.
- **Property catalogue**: properties, areas, locations, cities, GCC cities, builders, interiors and blogs, each a Payload collection in [src/collections/](src/collections/).
- **Search**: header search, suggestions, text search, facets and search results ([src/search/](src/search/), [src/properties/](src/properties/)). Search queries are logged to a `SearchStats` collection, with a retention job.
- **City context**: city-aware content and rankings ([src/city/](src/city/)).
- **Lead capture and CRM**: configurable forms, lead intake, scoring, assignment, activity log, consent, CSV export, email notifications and auto-replies (Resend), Cloudflare Turnstile spam check ([src/forms/](src/forms/), [src/leads/](src/leads/)).
- **WhatsApp advisor and Advisory page** ([src/whatsapp/](src/whatsapp/), [src/advisory/](src/advisory/)).
- **SEO**: SEO plugin, redirects plugin, sitemaps, `robots.txt`, `llms.txt`, JSON-LD, and a "stealth mode" (noindex) for staging.
- **PWA**: web manifest and generated icons ([src/pwa/](src/pwa/)).
- **Content tools**: draft preview and live preview from the admin, role-based access control ([src/access/](src/access/)), Cloudflare R2 media storage.
- **Page caching and revalidation**: cached pages revalidated on content change, or manually with `pnpm revalidate`.

## 3. Tech Stack

| Area            | Technology                                                                   |
| --------------- | ---------------------------------------------------------------------------- |
| Framework       | Next.js 16 (App Router), React 19                                            |
| CMS             | Payload CMS 3.90 (Lexical rich text, SEO and redirects plugins)              |
| Database        | MongoDB (`@payloadcms/db-mongodb`)                                           |
| Media storage   | Cloudflare R2 via `@payloadcms/storage-s3` (local `public/media` when unset) |
| Email           | Resend (`@payloadcms/email-resend`); printed to console when unset           |
| Styling         | Tailwind CSS 4, Radix UI, shadcn-style `components.json`, lucide-react icons |
| Animation       | framer-motion / motion, GSAP, Lenis                                          |
| Forms and state | react-hook-form, Zod, Zustand                                                |
| Language        | TypeScript 5.9                                                               |
| Testing         | Vitest (integration), Playwright (end-to-end), Testing Library               |
| Tooling         | pnpm 10, ESLint 9, Prettier, Husky, lint-staged, commitlint                  |
| Deployment      | Vercel (`vercel.json`), plus a `Dockerfile`; CI in `.gitlab-ci.yml`          |

## 4. File Structure

```
pattem-estates-payload/
├── src/
│   ├── app/
│   │   ├── (frontend)/      # Public site: pages, API routes (search, leads, forms…), sitemaps
│   │   └── (payload)/       # Payload admin and REST/GraphQL routes
│   ├── collections/         # Payload collections (Pages, Properties, Locations, Leads, Users…)
│   ├── globals/             # Site Settings, Search Page
│   ├── blocks/              # Page-builder blocks + RenderBlocks.tsx
│   ├── Header/ Footer/ Hero/ Modal/   # Layout pieces (component + CMS config)
│   ├── leads/ forms/        # Lead intake, scoring, emails, jobs; dynamic forms
│   ├── search/ properties/  # Search, suggestions, facets, property helpers
│   ├── city/ locations/ builders/ interiors/ advisory/ qa/   # Domain modules
│   ├── access/              # Role-based access control
│   ├── plugins/             # Payload plugin setup (SEO, redirects, R2 storage)
│   ├── seed/                # Default data seeding (also forms/seed, city, builders, advisory)
│   ├── components/ designSystem/ styles/ icons/   # Shared UI and styling
│   ├── payload.config.ts    # Main Payload config
│   ├── payload-types.ts     # Generated (gitignored)
│   └── proxy.ts             # Next.js request proxy
├── tests/
│   ├── int/                 # Vitest integration tests (*.int.spec.ts)
│   ├── e2e/                 # Playwright tests (*.e2e.spec.ts)
│   └── helpers/             # Login and seed-user helpers
├── scripts/                 # verify-env, revalidate, PWA icons, property-mock check
├── docs/                    # Project docs: setup, deployment, development guides
├── public/                  # Static assets
├── next.config.ts  vitest.config.mts  playwright.config.ts  tsconfig.json
├── Dockerfile  docker-compose.yml  vercel.json  .gitlab-ci.yml
└── .env.example             # Environment variable template
```

## 5. Setup Instructions

**Prerequisites**: Node.js `^20.9.0` or `>=22`, pnpm 9/10/11, Docker (recommended, for local MongoDB).

```bash
git clone git@gitlab.com:pattemdigital-group/pattem-estates-payload.git
cd pattem-estates-payload

cp .env.example .env         # set PAYLOAD_SECRET and PREVIEW_SECRET (openssl rand -base64 32)
docker compose up -d mongo   # local MongoDB (or point DATABASE_URI at your own)
pnpm install                 # also installs Husky git hooks
pnpm generate                # required: types and import map are gitignored
pnpm seed                    # default homepage and site settings
pnpm dev
```

Open `http://localhost:3000/admin`, create the first user and give them the **admin** role.

**Environment variables** (see `.env.example` for the full list). Locally only these need values:

| Variable                 | Purpose                                                           |
| ------------------------ | ----------------------------------------------------------------- |
| `DATABASE_URI`           | MongoDB connection (default `mongodb://127.0.0.1/pattem-estates`) |
| `PAYLOAD_SECRET`         | Payload secret                                                    |
| `PREVIEW_SECRET`         | Draft preview links                                               |
| `NEXT_PUBLIC_SERVER_URL` | Site origin, `http://localhost:3000`                              |

Optional: `RESEND_API_KEY` (email), `R2_*` (media storage), `TURNSTILE_*` (form spam check), `NEXT_PUBLIC_STEALTH_MODE` (noindex), `NEXT_PUBLIC_GTM_ID`. `NEXT_PUBLIC_*` values are baked in at build time. Staging and production values: [README.md](README.md) and [docs/deployment/environment-variables.md](docs/deployment/environment-variables.md).

More detail: [docs/development/setup.md](docs/development/setup.md), [CONTRIBUTING.md](CONTRIBUTING.md).

## 6. Main Scripts

| Script                                                                                      | What it does                                                                       |
| ------------------------------------------------------------------------------------------- | ---------------------------------------------------------------------------------- |
| `pnpm dev`                                                                                  | Start the dev server                                                               |
| `pnpm build`                                                                                | Production build (needs MongoDB reachable)                                         |
| `pnpm start`                                                                                | Serve the production build                                                         |
| `pnpm dev:prod`                                                                             | Clean `.next`, build, then start                                                   |
| `pnpm generate`                                                                             | `generate:types` + `generate:importmap` (run after changing collections or blocks) |
| `pnpm seed`                                                                                 | Seed default homepage and settings (`-- --force` to re-seed)                       |
| `pnpm seed:forms` / `seed:cities` / `seed:qa` / `seed:builders-page` / `seed:advisory-page` | Seed forms, cities, QA data, builders page, advisory page                          |
| `pnpm revalidate`                                                                           | Revalidate cached pages                                                            |
| `pnpm leads:import-legacy`                                                                  | Import legacy lead submissions                                                     |
| `pnpm lint` / `lint:fix`                                                                    | ESLint                                                                             |
| `pnpm format` / `format:check`                                                              | Prettier                                                                           |
| `pnpm type-check`                                                                           | `tsc --noEmit`                                                                     |
| `pnpm check:property-mocks`                                                                 | Guard script against mock property data                                            |
| `pnpm verify:env`                                                                           | Validate an env file before deploying                                              |
| `pnpm quality`                                                                              | Full gate: generate, lint, type-check, mock check, integration tests, build        |
| `pnpm payload`                                                                              | Payload CLI                                                                        |

Git hooks (Husky): `pre-commit` runs `pnpm generate` and lint-staged; `commit-msg` enforces conventional commits (commitlint); `pre-push` runs type check, lint, affected tests and the build as needed.

## 7. Running Tests

| Command         | Runs                                                             |
| --------------- | ---------------------------------------------------------------- |
| `pnpm test:int` | Vitest integration tests in `tests/int/**/*.int.spec.ts` (jsdom) |
| `pnpm test:e2e` | Playwright tests in `tests/e2e/` (Chromium)                      |
| `pnpm test`     | Both, integration first                                          |

Notes:

- **Integration tests need MongoDB.** Run `docker compose up -d mongo` and keep `DATABASE_URI` pointing at a local or throwaway database. They create and delete documents, and [vitest.setup.ts](vitest.setup.ts) refuses to run if the database name contains `stag` or `prod`.
- **E2E tests** start the app themselves (`pnpm build && pnpm start`, up to 5 minutes) or reuse a server already on port 3000. Playwright's browser must be installed once: `pnpm exec playwright install chromium`.
- Run a single integration file: `pnpm test:int tests/int/slug.int.spec.ts`.
- Run only tests affected by changes: `pnpm test:int --changed main`.
- Before opening a merge request, run `pnpm quality`.

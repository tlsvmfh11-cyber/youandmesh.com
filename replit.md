# Overview

This is a full-stack TypeScript web application using React on the frontend and Express on the backend. The project follows a monorepo-style structure with shared code between client and server. Currently it appears to be a starter/template project with minimal business logic — it has a basic user schema and in-memory storage, with the frontend router showing only a 404 page. The root route serves a static HTML file (`client/public/site.html`).

# User Preferences

Preferred communication style: Simple, everyday language.

# System Architecture

## Directory Structure
- `client/` — React frontend (Vite-powered SPA)
- `server/` — Express backend API server
- `shared/` — Shared code (database schema, types) used by both client and server
- `migrations/` — Drizzle ORM database migrations
- `script/` — Build scripts

## Frontend Architecture
- **Framework:** React with TypeScript
- **Bundler:** Vite (with HMR in development via `server/vite.ts`)
- **Routing:** Wouter (lightweight client-side router)
- **State/Data Fetching:** TanStack React Query for server state management
- **UI Components:** shadcn/ui (new-york style) built on Radix UI primitives
- **Styling:** Tailwind CSS with CSS variables for theming (light/dark mode support)
- **Forms:** React Hook Form with Zod resolvers via `@hookform/resolvers`
- **Path aliases:** `@/` maps to `client/src/`, `@shared/` maps to `shared/`

## Backend Architecture
- **Framework:** Express 5 running on Node.js
- **Language:** TypeScript, executed with `tsx`
- **API Pattern:** REST API with routes registered in `server/routes.ts`
- **Storage Layer:** Abstracted via `IStorage` interface in `server/storage.ts`. Currently uses in-memory storage (`MemStorage`), but designed to be swapped for database-backed storage.
- **Development:** Vite dev server is integrated as middleware for HMR
- **Production:** Client is built to `dist/public/`, server is bundled with esbuild to `dist/index.cjs`

## Database
- **ORM:** Drizzle ORM with PostgreSQL dialect
- **Schema:** Defined in `shared/schema.ts` — currently has a `users` table with `id` (UUID), `username`, and `password`
- **Validation:** Zod schemas auto-generated from Drizzle schema via `drizzle-zod`
- **Migrations:** Managed with `drizzle-kit push` (schema push approach, not migration files)
- **Connection:** Requires `DATABASE_URL` environment variable pointing to a PostgreSQL database
- **Note:** The current runtime storage is in-memory (`MemStorage`). When implementing features that need persistence, switch to using Drizzle with the PostgreSQL database.

## API Request Pattern
- The frontend uses a custom `apiRequest` helper in `client/src/lib/queryClient.ts` for making API calls
- Credentials are included with requests (`credentials: "include"`)
- React Query is configured with `staleTime: Infinity` and no automatic refetching
- 401 handling can be configured per-query to either throw or return null

## Build System
- **Dev:** `npm run dev` — runs Express + Vite dev server together with HMR
- **Build:** `npm run build` — builds client with Vite, bundles server with esbuild
- **Start:** `npm run start` — runs production build from `dist/`
- **DB Push:** `npm run db:push` — pushes schema changes to database

# External Dependencies

## Database
- **PostgreSQL** — Required, connected via `DATABASE_URL` environment variable
- **Drizzle ORM** — Query builder and schema management
- **connect-pg-simple** — PostgreSQL session store (available but not yet wired up)

## Key NPM Packages
- **Express 5** — HTTP server
- **Vite** — Frontend build tool and dev server
- **React 18** — UI framework
- **TanStack React Query** — Async state management
- **Radix UI** — Accessible UI primitives (full suite installed)
- **Tailwind CSS** — Utility-first CSS framework
- **Zod** — Schema validation
- **Wouter** — Client-side routing
- **Lucide React** — Icon library
- **Recharts** — Charting library (via shadcn chart component)
- **embla-carousel-react** — Carousel component

## Replit-Specific Plugins
- `@replit/vite-plugin-runtime-error-modal` — Runtime error overlay
- `@replit/vite-plugin-cartographer` — Dev tooling (dev only)
- `@replit/vite-plugin-dev-banner` — Dev banner (dev only)

## Potentially Available (in build allowlist but not necessarily active)
- **OpenAI / Google Generative AI** — AI service integrations
- **Stripe** — Payment processing
- **Passport / passport-local** — Authentication
- **Nodemailer** — Email sending
- **Multer** — File upload handling
- **jsonwebtoken** — JWT authentication
- **ws** — WebSocket support
- **xlsx** — Spreadsheet processing
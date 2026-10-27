# C4R / C4C Transition Support

A responsive directory and lead-capture app for residential and healthcare transition support.

## Run & Operate

- `pnpm --filter @workspace/api-server run dev` — run the API server (port 5000)
- `pnpm run typecheck` — full typecheck across all packages
- `pnpm run build` — typecheck + build all packages
- `pnpm --filter @workspace/api-spec run codegen` — regenerate API hooks and Zod schemas from the OpenAPI spec
- `pnpm --filter @workspace/db run push` — push DB schema changes (dev only)
- Required env: `DATABASE_URL` — Postgres connection string
- Required staff access configuration: `C4R_ADMIN_EMAILS` — comma-separated email addresses allowed to view `/admin`
- Clerk Auth is provisioned through Replit; Clerk keys are managed automatically.

## Stack

- pnpm workspaces, Node.js 24, TypeScript 5.9
- API: Express 5
- DB: PostgreSQL + Drizzle ORM
- Validation: Zod (`zod/v4`), `drizzle-zod`
- API codegen: Orval (from OpenAPI spec)
- Build: esbuild (CJS bundle)

## Where things live

- `artifacts/c4r-transition-support` — public directory, intake flow, reservation modal, staff lead view
- `artifacts/api-server/src/routes/transition.ts` — listing, intake, reservation, and staff lead endpoints
- `lib/api-spec/openapi.yaml` — API source of truth
- `lib/db/src/schema/leads.ts` — persistent intake and reservation tables

## Architecture decisions

- Visitor access is captured before revealing the sample directory; intake IDs are preserved for the active browser session.
- Lead submissions use PostgreSQL and reservations reference their initial intake.
- Clerk authentication is required for staff leads, with an additional server-side email allowlist (`C4R_ADMIN_EMAILS`).
- Directory entries are illustrative examples and do not represent verified availability.

## Product

Families can unlock sample residential and healthcare transition listings, request information, and select a preferred contact method. Authorized staff can review intake and reservation submissions at `/admin`.

## User preferences

- Keep the experience professional, trustworthy, modern, and frictionless on mobile and desktop.

## Gotchas

- Set `C4R_ADMIN_EMAILS` to one or more authorized staff email addresses; without it, the API denies all staff lead access.
- Run `pnpm --filter @workspace/api-spec run codegen` after changing the OpenAPI contract.

## Pointers

- See the `pnpm-workspace` skill for workspace structure, TypeScript setup, and package details

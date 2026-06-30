# CLAUDE.md

This file provides guidance to Claude Code (claude.ai/code) when working with code in this repository.

@AGENTS.md

## Commands

```bash
npm run dev      # start dev server (Turbopack, http://localhost:3000)
npm run build    # production build (Turbopack)
npm run start    # run production build
npm run lint     # run ESLint (calls `eslint` directly — NOT `next lint`)
```

There are no tests configured yet.

## Stack

- **Next.js 16.2.9** with App Router (see breaking changes below)
- **React 19**
- **TypeScript 5** (strict mode, path alias `@/*` → `./`)
- **Tailwind CSS v4** via `@tailwindcss/postcss` — no `tailwind.config.js`
- **Geist** fonts loaded via `next/font/google`

## Architecture

All routes live under `app/` using the App Router file-system convention:
- `app/layout.tsx` — root layout, sets fonts and `<html>`/`<body>` wrappers
- `app/page.tsx` — home route (`/`)
- `app/globals.css` — global styles; Tailwind is loaded with `@import "tailwindcss"` (v4 syntax)
- `public/assets/` — static brand assets (logos, branding images)

## Next.js 16 Breaking Changes

These differ from most AI training data — heed the AGENTS.md instruction to read `node_modules/next/dist/docs/` before writing code.

| Area | Old (≤15) | New (16) |
|---|---|---|
| Linting | `next lint` / `npx next lint` | `eslint` CLI directly; `next lint` is removed |
| ESLint config | `.eslintrc` | Flat config (`eslint.config.mjs`) — already in place |
| Bundler | Webpack opt-in via `--turbopack` | **Turbopack is default** for both `dev` and `build` |
| Middleware | `middleware.ts` in project root | Renamed to `proxy.ts`; export `proxy` function or default |
| Runtime config | `serverRuntimeConfig` / `publicRuntimeConfig` | Removed — use `process.env` / `NEXT_PUBLIC_` env vars |
| Caching flags | `experimental.dynamicIO`, `experimental.useCache` | Replaced by top-level `cacheComponents: true` in `next.config.ts` |
| AMP | `next/amp`, `export const config = { amp: true }` | Removed entirely |
| Scroll override | Next.js overrode `scroll-behavior` on nav | No longer overridden by default; opt in with `data-scroll-behavior="smooth"` on `<html>` |
| Build metrics | `size` / `First Load JS` in build output | Removed — use Lighthouse or Vercel Analytics |

### Tailwind v4 Specifics
- Import with `@import "tailwindcss"` (not the old `@tailwind base/components/utilities` directives)
- No `tailwind.config.js` required — configuration happens in CSS via `@theme`
- PostCSS plugin is `@tailwindcss/postcss`, not `tailwindcss`

### Proxy (formerly Middleware)
Create `proxy.ts` at the project root (not `middleware.ts`):
```ts
import { NextResponse } from 'next/server'
import type { NextRequest } from 'next/server'

export function proxy(request: NextRequest) { ... }

export const config = { matcher: '/protected/:path*' }
```

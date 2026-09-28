# Project Structure

Next.js (App Router) project organized by **feature (domain)**. `app/` handles routing only; business logic lives in `features/`.

## Folder Tree

```
project-root/
├── src/
│   ├── app/                          # Routing only (thin pages)
│   │   ├── (auth)/
│   │   │   ├── login/page.tsx
│   │   │   ├── register/page.tsx
│   │   │   └── layout.tsx
│   │   ├── (public)/
│   │   │   ├── page.tsx              # Home
│   │   │   ├── blog/
│   │   │   │   ├── page.tsx
│   │   │   │   └── [slug]/page.tsx
│   │   │   └── layout.tsx
│   │   ├── (dashboard)/
│   │   │   ├── admin/
│   │   │   │   ├── page.tsx
│   │   │   │   ├── users/page.tsx
│   │   │   │   └── posts/page.tsx
│   │   │   ├── author/
│   │   │   │   ├── page.tsx
│   │   │   │   └── posts/
│   │   │   │       ├── page.tsx
│   │   │   │       └── new/page.tsx
│   │   │   ├── user/page.tsx
│   │   │   └── layout.tsx            # Sidebar + session check
│   │   ├── api/                      # Route handlers (webhooks, etc.)
│   │   ├── layout.tsx
│   │   ├── globals.css
│   │   ├── not-found.tsx
│   │   ├── error.tsx
│   │   ├── loading.tsx
│   │   └── favicon.ico
│   │
│   ├── features/                     # Business logic (domain-wise)
│   │   ├── auth/
│   │   │   ├── actions/              # Server actions
│   │   │   ├── components/           # LoginForm, RegisterForm
│   │   │   ├── services/             # DB / API calls
│   │   │   ├── schemas/              # Zod validation
│   │   │   ├── types/
│   │   │   └── index.ts              # Public API of the feature
│   │   ├── posts/
│   │   ├── users/
│   │   └── comments/
│   │
│   ├── components/                   # Shared UI only (no business logic)
│   │   ├── ui/                       # shadcn/ui components
│   │   ├── layout/                   # Navbar, Sidebar, Footer
│   │   └── shared/                   # Logo, EmptyState, Loader
│   │
│   ├── lib/                          # Configured tools + helpers
│   │   ├── db.ts
│   │   ├── auth.ts
│   │   ├── utils.ts
│   │   └── env.ts
│   │
│   ├── hooks/                        # Shared client hooks
│   ├── types/                        # Global types
│   ├── constants/                    # Routes, roles, site config
│   └── middleware.ts                 # Role-based route protection
│                                     # (proxy.ts in Next 16)
│
├── public/
├── .env.example
├── components.json
├── next.config.ts
├── tsconfig.json
├── eslint.config.mjs
├── AGENTS.md / CLAUDE.md
└── README.md
```

## Where Does Code Go?

| Code type | Location |
|---|---|
| Route / page (thin) | `src/app/` |
| Logic for a single domain | `src/features/<name>/` |
| Reusable UI, no business logic | `src/components/` |
| Config, clients, helpers | `src/lib/` |
| Shared hooks | `src/hooks/` |
| Global types | `src/types/` |
| Routes, roles, site config | `src/constants/` |

## Rules

1. `app/` is routing only. Pages compose feature components and contain no logic.
2. Server actions live inside `features/<name>/actions/` with `"use server"`.
3. Dependency direction: `app → features → components / lib / hooks`.
4. `components/` never imports from `features/`.
5. Features are imported through their `index.ts` only, never through internals.
6. Zod schemas live in `features/<name>/schemas/` and are shared by client and server.
7. Role protection uses two layers: `middleware.ts` and the dashboard `layout.tsx`.
8. Path alias: `@/*` maps to `src/*`.

## Naming

| Item | Convention | Example |
|---|---|---|
| Folders | `kebab-case` | `author-posts` |
| Components | `PascalCase` | `PostCard.tsx` |
| Hooks | `useSomething` | `useAuth.ts` |
| Route groups | `(name)` | `(dashboard)` |

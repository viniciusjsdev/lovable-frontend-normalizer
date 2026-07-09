# Version Baseline

## Preserve the Existing Stack

Preserve the project stack unless the user explicitly asks for upgrades or replacements.

Preferred Lovable-oriented baseline:

- React 19.x
- TypeScript 5.x
- Vite 8.x
- TanStack Router 1.x
- TanStack Start 1.x
- Tailwind CSS 4.x
- Zustand 5.x
- React Hook Form 7.x
- Zod 3.x
- Radix UI or shadcn-style primitives
- Lucide React
- Recharts
- Sonner

If the project uses a slightly different Lovable stack, inspect it first and preserve it unless there is a concrete build or compatibility reason to change.

## Do Not Replace Without Request

Do not replace the current stack with:

- Next.js
- React Router
- CRA or CRACO
- JavaScript/JSX when the project is TypeScript
- Redux
- Material UI
- Chakra
- Bootstrap
- Styled Components
- PrimeReact
- SCSS as the primary styling system
- another design system

Make such changes only when the user explicitly requests them or when a concrete issue makes the current choice untenable.

## Stack Migration Is Separate

Treat conversion to another repository's stack as a migration, not normal frontend normalization.

Examples of migrations:

- TanStack Router to React Router DOM
- TanStack Start SSR/file routes to CRA/CRACO SPA
- TypeScript to JavaScript/JSX
- Tailwind/shadcn/Radix to PrimeReact/SCSS
- Lucide icons to PrimeIcons or Bootstrap Icons

Do not perform these changes implicitly. If the user asks for them, first state the tradeoff: UI/UX can be preserved only with deliberate visual comparison because rendering, styling, routing and hydration behavior can change.

## Package Manager Standard

App Factory standard is npm.

For Lovable-generated or App Factory MVP projects, normalize to npm unless the user explicitly asks to keep another manager or the project has a documented constraint requiring pnpm, yarn or bun.

This is an intentional factory standard, not a validation workaround.

When normalizing to npm:

- remove `pnpm-lock.yaml`
- remove `yarn.lock`
- remove `bun.lock`
- remove `bun.lockb`
- keep `package-lock.json` if present
- run `npm install` to create or update `package-lock.json`
- use npm for install, format, lint, build and dev-server commands
- report the package manager normalization in the final summary

Do not keep multiple lockfiles. Do not repeatedly run install commands with different managers. Do not patch `node_modules` manually with downloaded packages.

If the project must keep pnpm, yarn or bun, document the exception and use that manager consistently for all validation commands.

If `node_modules` already exists, npm validation may still run. If local binaries or package files are missing or stale, classify validation as blocked by incomplete dependencies unless a clean npm install resolves it.

## Naming

User-facing labels may stay in the product language.

Code identifiers should be consistent and preferably in English unless the project already uses another clear convention.

Adapt names to the actual project domain. Do not force finance-specific, SaaS-specific or app-specific names into unrelated projects.

---
name: lovable-frontend-normalizer
description: Use this skill when a project has frontend code generated or accelerated by Lovable and the user wants to normalize, polish, refactor, standardize architecture, improve component responsibilities, prepare API boundaries, improve mobile-first behavior, preserve UI/UX, or make the frontend more production-ready. This skill is generic and must not depend on any specific product, brand, app name, domain, or business model.
---

# Lovable Frontend Normalizer

## Purpose

Normalize frontend code generated or accelerated by Lovable into a maintainable React/TypeScript architecture while preserving the existing UI/UX.

Lovable may generate screens, layouts, flows, mock data, components, interactions, copy and visual identity quickly. Treat that generated experience as product work to preserve. Improve architecture, file organization, responsibilities, maintainability, mobile behavior and backend readiness without redesigning the product.

Core rule: preserve first, normalize second.

Do not discard, remove, simplify, rewrite or replace UI/UX created by Lovable unless the user explicitly asks for it, the code is broken, the behavior is inaccessible or unusable, the implementation prevents the app from building or running, or duplicated dead code has no user-facing effect.

## Load References

Read the relevant references before making changes:

- `references/version-baseline.md` for stack preservation and package-manager rules.
- `references/architecture-standard.md` before moving files, routes or feature modules.
- `references/component-responsibilities.md` before splitting pages, components, hooks, services or shell/layout files.
- `references/mock-to-api-boundary.md` before touching mock data, services, DTOs or future API contracts.
- `references/mobile-first-checklist.md` before changing layout, navigation, tables, charts, forms, dialogs or responsive classes.
- `references/local-run-checklist.md` before validation or dev-server work.
- `references/validation-checklist.md` before finishing.

Run `scripts/scan-lovable-frontend.mjs` from the consuming project when the script is available in that project.

## Workflow

1. Inspect `package.json`, lockfiles, `src/routes`, `src/components`, `src/pages`, `src/features`, `src/mocks` and shared UI folders.
2. Preserve the current React/TypeScript/TanStack/Tailwind stack unless the user asks for a change.
3. Normalize App Factory Lovable projects to npm unless the user or project docs explicitly require pnpm, yarn or bun.
4. If npm normalization is allowed, remove conflicting lockfiles (`pnpm-lock.yaml`, `yarn.lock`, `bun.lock`, `bun.lockb`), keep or generate `package-lock.json`, and use npm for install, format, lint, build and dev-server commands.
5. If the project explicitly requires a non-npm package manager, use it and report that npm normalization was intentionally skipped.
6. Do not switch managers repeatedly or repair `node_modules` manually. If dependency state is broken, report the blocker and the closest completed validation.
7. Identify overloaded route files, large page/component files, direct mock imports, direct API calls in UI, large shell/layout files and scattered generated data.
8. Create a concise refactor plan that states what will be moved, what UI/UX will be preserved and whether npm normalization will happen.
9. Keep route files thin: route declaration, metadata, params/search validation and rendering a feature page.
10. Move screen composition into feature pages when needed.
11. Move reusable screen sections into feature components.
12. Move state orchestration, derived data and side effects into hooks.
13. Move data access, DTO mapping and mock/real switching into services.
14. Isolate mocks behind services or feature data boundaries.
15. Preserve visual identity, copy, flows, interactions, navigation, cards, forms, charts, dialogs, drawers, animations, toasts, loading states, empty states and existing demo behavior.
16. Remove generated platform metadata only when it is not required for build/runtime and the project convention allows it.
17. Keep mobile-first behavior working at 360px width unless the product explicitly targets desktop only.
18. Update architecture/API documentation when the refactor creates or clarifies boundaries.
19. Run format, lint, build, architecture scan and local dev-server validation when possible.
20. Report exactly what was changed, what was preserved, package manager changes and which validations passed or failed.

## Architecture Target

Use feature-based frontend architecture. Create only folders that the current code needs; do not over-engineer small projects.

Preferred shape:

```txt
src/
  app/
    providers.tsx
    router.tsx
    config.ts
  routes/
  features/
    feature-name/
      FeaturePage.tsx
      components/
      hooks/
      services/
      schemas/
      types.ts
      index.ts
  shared/
    components/
    ui/
    hooks/
    lib/
    utils/
    types/
  services/
    api-client.ts
  stores/
  mocks/
    data/
    services/
    factories/
```

Example thin TanStack route:

```tsx
import { createFileRoute } from "@tanstack/react-router";
import { HomePage } from "@/features/home";

export const Route = createFileRoute("/")({
  component: HomePage,
});
```

## Preservation Rules

Preserve:

- visual identity, colors, typography, spacing, icons, layout direction and animations
- user-facing copy, product language, screen flows and navigation structure
- mobile navigation, modals, drawers, cards, forms, charts and data visualizations
- loading, error, empty and success states
- mock/demo behavior, toasts, feedback and interactions

Do not replace rich generated UI with generic placeholders. Do not remove screens because they look duplicated. Do not remove mock data because there is no backend yet. If a component is too large, split it while preserving the rendered output.

## Stack Migration

Normalizing is not stack migration.

Do not convert TypeScript to JavaScript, TanStack Router to React Router, Vite/TanStack Start to CRA/CRACO, Tailwind/shadcn/Radix to PrimeReact/SCSS, or Lucide to another icon library unless the user explicitly asks for a stack migration.

If the user asks to align with another repository stack, explain that this is a separate migration. Preserve UI/UX only with explicit visual comparison, because changing the stack can alter rendering, routing, SSR/hydration behavior, styling and component semantics.

## Package Manager Standard

App Factory standard is npm.

For Lovable-generated or App Factory MVP projects, npm normalization is part of frontend normalization, not an ad hoc fallback. Use npm unless the user explicitly asks to keep another manager or the project has a documented constraint requiring pnpm, yarn or bun.

When normalizing to npm:

- remove `pnpm-lock.yaml`, `yarn.lock`, `bun.lock` and `bun.lockb`
- keep `package-lock.json` when present
- run `npm install` to create or update `package-lock.json`
- run validation with `npm run format`, `npm run lint`, `npm run build` and `npm run dev`
- report that the package manager was normalized to npm

Do not delete or rewrite dependency declarations in `package.json` solely because the package manager changed. Do not manually patch `node_modules`. If stale `node_modules` causes validation failures, report it clearly; remove/reinstall dependencies only when that is appropriate for the current project and not mixed with unrelated source changes.

## Boundaries

Use this direction:

```txt
Route -> Feature Page -> Hook/View Model -> Service -> Mock or API
```

Avoid:

```txt
Route -> Mock
Component -> Mock
Chart -> Mock
Form -> Fake persistence directly
```

Services are the data boundary. Components render UI. Hooks orchestrate state and side effects. Routes declare routing.

## Validation

Attempt the npm commands after package manager normalization:

```bash
npm install
npm run format
npm run lint
npm run build
node .agents/skills/lovable-frontend-normalizer/scripts/scan-lovable-frontend.mjs
npm run dev
```

Adapt the script path if the skill is installed elsewhere, such as `node ~/.codex/skills/lovable-frontend-normalizer/scripts/scan-lovable-frontend.mjs`.

Do not claim the project runs locally unless the dev server was actually started and checked for immediate startup errors. If validation is blocked by missing environment variables, unavailable ports, missing commands, incomplete dependencies or environment limitations, say that clearly and report the closest completed validation.

## Definition of Done

Finish only after these are true or explicitly reported as blocked:

- existing UI/UX and screens are preserved
- routes touched by the change are thin
- touched screens have appropriate feature folders
- components, hooks, services and mocks have clear boundaries
- shared components remain domain-neutral
- shell/layout files are not dumping grounds
- mobile-first behavior is preserved
- docs are updated where architecture or API boundaries changed
- package manager is npm, or a documented exception explains why npm was skipped
- format, lint, build, architecture scan and local dev-server validation were attempted

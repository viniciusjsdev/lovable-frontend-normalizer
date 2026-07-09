# Local Run Checklist

The normalized project must be tested locally whenever possible.

## Package Manager Standard

App Factory frontend normalization uses npm by default.

Before validation:

- remove `pnpm-lock.yaml`, `yarn.lock`, `bun.lock` and `bun.lockb` unless a documented project exception requires them
- keep or generate `package-lock.json`
- do not keep multiple lockfiles
- use npm for install, format, lint, build and dev-server commands

## Required Checks

Run the equivalent of:

```bash
npm install
npm run format
npm run lint
npm run build
node .agents/skills/lovable-frontend-normalizer/scripts/scan-lovable-frontend.mjs
npm run dev
```

Adapt only the scanner path to the installed skill path. Do not adapt npm commands to another package manager unless the user or project docs explicitly require the exception.

## Package Manager Exceptions

Use pnpm, yarn or bun only when the user explicitly asks for it or the project documents a hard requirement.

When an exception exists:

- document why npm normalization was skipped
- use the required manager consistently
- do not create npm lockfiles as a side effect
- report validation as blocked if the required manager is unavailable

Rules for all cases:

- Do not run multiple package managers trying to make one pass.
- Do not repair `node_modules` by manually unpacking packages or editing dependency folders.

If dependencies are already installed, direct local binary execution is acceptable for diagnosis, for example `node node_modules/vite/bin/vite.js build`, but only if the dependency tree is complete enough to run.

## Dev Server Validation

The agent must:

1. Start the local dev server.
2. Wait for the ready/local URL output.
3. Confirm no immediate runtime startup error.
4. Open or request the local URL if the environment allows it.
5. Stop the server.
6. Report the command and result.

Do not say "it works locally" unless the local dev server actually started.

## Reporting

If local running is impossible, report why:

- missing env vars
- missing dependencies
- unavailable port
- unsupported environment
- package manager exception or npm install issue
- incomplete `node_modules`
- command missing
- tool limitation

Also report the closest completed validation.

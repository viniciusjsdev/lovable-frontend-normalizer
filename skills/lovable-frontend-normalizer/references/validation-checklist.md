# Validation Checklist

Before finishing, check:

- [ ] npm is used as the package manager or a documented exception explains why not
- [ ] conflicting pnpm/yarn/bun lockfiles removed when npm normalization applies
- [ ] `package-lock.json` exists or npm install failure is reported
- [ ] dependencies installed or already available through npm
- [ ] format executed
- [ ] lint executed
- [ ] build executed
- [ ] architecture scan executed
- [ ] local dev server started
- [ ] startup errors checked
- [ ] UI/UX preserved
- [ ] no screens removed
- [ ] routes are thin
- [ ] feature folders exist for touched screens
- [ ] mocks isolated behind services
- [ ] components have clear responsibilities
- [ ] shared components are domain-neutral
- [ ] shell/layout is not overloaded
- [ ] mobile-first behavior preserved
- [ ] documentation updated when boundaries changed

Classify failures as:

- pre-existing issue
- introduced by refactor
- missing dependency
- incomplete dependency installation
- npm install failure
- documented package manager exception unavailable
- missing environment variable
- unsupported environment
- command not available

Fix introduced failures before finishing.

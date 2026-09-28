# AGENTS.md

## Cursor Cloud specific instructions

This is the `@sanity/color` repository, structured as a pnpm monorepo: the
published `@sanity/color` package (the Sanity color palette) lives in
`packages/color`, the Figma plugin that syncs the palette to Figma color styles
and variables in `packages/figma-color`, and the Storybook app in
`apps/storybook` (`pnpm-workspace.yaml`). The root `package.json` is a private
workspace root whose scripts orchestrate via pnpm filters. Package manager is
pnpm (`packageManager` pin in `package.json`); developing in this repo requires
Node `>=22.13`. The tooling mirrors the
[sanity-io/ui](https://github.com/sanity-io/ui) monorepo, so keep dependency
versions, lint/format config and CI workflows in step with it.

Standard scripts live in the root `package.json` (`lint`, `test`, `build`,
`dev`). Notes that are not obvious from the scripts:

- Linting uses [oxlint](https://oxc.rs/docs/guide/usage/linter.html) with a
  root `.oxlintrc.json` (type-aware via `oxlint-tsgolint`). TypeScript type
  checking is included in `pnpm lint` via the `typeCheck` option — there is no
  separate `tsc` command. Run `pnpm lint:fix` to auto-fix issues when possible.
  Suppressions use `oxlint-disable-next-line` comments. Storybook-specific
  rules come from `eslint-plugin-storybook`, loaded through oxlint's JS plugins
  support and enabled via config `overrides` scoped to story files and
  `.storybook/main.ts`. A clean run prints nothing.
- `pnpm knip` runs [knip](https://knip.dev) (config in `knip.jsonc`, also a CI
  job) to detect unused files, dependencies and exports. The script passes
  `--treat-config-hints-as-errors`, so stale knip config also fails the run.
- Packages are built with [tsdown](https://tsdown.dev) via
  `@sanity/tsdown-config` (`tsdown.config.mts`). In the workspace,
  `@sanity/color` resolves directly to TypeScript source through its dev
  `exports`; the publishable `exports` (dist `import`/`require`) live under
  `publishConfig` and are applied by `pnpm pack`/`publish`.
- `packages/color/src/color.ts` is generated from `packages/color/src/config.ts`:
  regenerate it with `pnpm --filter @sanity/color generate` after changing the
  palette config; never edit it by hand.
- `pnpm test` runs the `@sanity/color` unit tests with vitest against source,
  so no build is required first.
- `pnpm dev` starts Storybook (`apps/storybook`) on http://localhost:6006. It
  resolves `@sanity/color` to source (hot reload, no rebuild needed) and uses
  the published `@sanity/ui` from npm, whose stylesheet is loaded in
  `.storybook/preview.tsx` via `@sanity/ui/styles.css`.
- `pnpm test:browser` renders every story in headless Chromium via
  `@storybook/addon-vitest` and runs story `play` interactions. The
  Playwright-provided browser must be installed once via
  `pnpm --filter sanity-color-storybook exec playwright install chromium`.
- Releases are managed with Changesets: run `pnpm changeset` to add a changeset
  to a PR that should trigger a release. Merging to `main` opens/updates a
  "Version Packages" PR, and merging that publishes to npm from
  `.github/workflows/release.yml` via npm trusted publishing (OIDC, no npm
  token). Trusted publishing is pinned to that workflow filename, so don't
  rename it.

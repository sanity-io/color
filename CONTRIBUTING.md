# Contributing guidelines

This repository is a pnpm monorepo. The published `@sanity/color` package lives
in [`packages/color`](packages/color), the Figma plugin lives in
[`packages/figma-color`](packages/figma-color), and the Storybook lives in
[`apps/storybook`](apps/storybook).

## Getting started

```sh
pnpm install
pnpm build
pnpm test
```

Run `pnpm dev` to start Storybook (http://localhost:6006). Storybook resolves
`@sanity/color` from the package source, so edits to `packages/color/src`
hot-reload without a rebuild.

## Changing the palette

The palette is defined in `packages/color/src/config.ts`, and
`packages/color/src/color.ts` is generated from it. Never edit `color.ts` by
hand: update `config.ts`, then regenerate it with
`pnpm --filter @sanity/color generate`.

The `ColorTool` story in Storybook is an interactive editor for the palette.
Enable its `showCode` control to get a `config.ts` snippet of the edited
palette, ready to paste into the package.

## Testing

Unit tests are written with [vitest](https://vitest.dev) and live next to the
source in `packages/color/src`. Run them with `pnpm test` (or `pnpm test:watch`
in the package for watch mode). They run against the package source, so no
build is required.

Browser tests live in the Storybook app (`apps/storybook`) and use
[Storybook's Vitest addon](https://storybook.js.org/docs/writing-tests/integrations/vitest-addon):
every story is rendered as a smoke test in headless Chromium, and interaction
tests are written as story [`play` functions](https://storybook.js.org/docs/writing-stories/play-function).

Install the Playwright-provided browser once with
`pnpm --filter sanity-color-storybook exec playwright install chromium`, then run
`pnpm test:browser`. While developing, `pnpm dev` exposes the same tests
interactively through the testing panel in the Storybook UI.

## Releasing

Releases are managed with [Changesets](https://github.com/changesets/changesets).

When you make a change that should be released, add a changeset to your pull
request:

```sh
pnpm changeset
```

Once pull requests with changesets are merged into `main`, a "Version Packages"
pull request is opened (and kept up to date) that bumps the affected package
versions and updates their changelogs. Merging that pull request publishes the
packages to npm through the
[`Release` workflow](https://github.com/sanity-io/color/actions/workflows/release.yml),
which uses npm [Trusted Publishing](https://docs.npmjs.com/trusted-publishers)
(OIDC). Releases from `main` are published under the `latest` dist-tag.

# @sanity/color

pnpm workspace for the Sanity color palette and related tooling.

Published packages live under `packages/`. The Storybook lives under `apps/`.

## Packages

| Package                                             | Description                                  |
| --------------------------------------------------- | -------------------------------------------- |
| [`@sanity/color`](packages/color)                   | Color palette                                |
| [`figma-plugin-sanity-color`](packages/figma-color) | Figma plugin for the `@sanity/color` palette |

## Apps

| App                                | Description                                                                               |
| ---------------------------------- | ----------------------------------------------------------------------------------------- |
| [`apps/storybook`](apps/storybook) | Palette and color tool Storybook ([localhost:6006](http://localhost:6006) via `pnpm dev`) |

## Requirements

- Node.js `>=22.13`
- [pnpm](https://pnpm.io) `12` (pinned via `packageManager` in `package.json`)

## Getting started

```sh
pnpm install
pnpm build
pnpm test
```

### Development

```sh
pnpm dev          # Storybook at http://localhost:6006
```

In the workspace, `@sanity/color` resolves to TypeScript source through package `exports`, so
Storybook hot-reloads package edits without a rebuild.

### Common scripts

| Script              | What it does                                      |
| ------------------- | ------------------------------------------------- |
| `pnpm build`        | Build `@sanity/color` and the Figma plugin        |
| `pnpm test`         | Unit tests (`@sanity/color`)                      |
| `pnpm test:browser` | Storybook browser tests (Chromium via Playwright) |
| `pnpm lint`         | Lint + type-check (oxlint)                        |
| `pnpm format`       | Format with oxfmt                                 |
| `pnpm knip`         | Unused files / dependencies / exports             |
| `pnpm changeset`    | Add a changeset for a release                     |

## Contributing & releasing

See [CONTRIBUTING.md](CONTRIBUTING.md). Releases use
[Changesets](https://github.com/changesets/changesets): add a changeset on your
PR; merging to `main` opens a “Version Packages” PR that publishes to npm when
merged.

## License

MIT — see [LICENSE](LICENSE).

# @sanity/color Storybook

Storybook for the [`@sanity/color`](../../packages/@sanity/color) palette:

- **Colors** – the full palette, with WCAG contrast badges. Click a swatch to copy its hex value.
- **ColorTool** – an interactive editor for designing the palette. Enable `showCode` to get a
  `packages/@sanity/color/src/config.ts` snippet of the edited palette, then regenerate
  `src/color.ts` with `pnpm --filter @sanity/color generate`.

`@sanity/color` resolves to the package source, so edits to `packages/@sanity/color/src` hot-reload
without a rebuild. The stories are built with the published [`@sanity/ui`](https://www.npmjs.com/package/@sanity/ui).

## Storybook guidelines

- All stories must export either a named `Default` or `Basic` story.
- Avoid creating custom titles for stories - these should be inferred via folder structure alone.
- Where possible, stories should be kept as simple as possible with minimal custom / presentational props.
- Prefer setting component values via storybook [args](https://storybook.js.org/docs/react/writing-stories/args) instead of passing them manually in props.

## Things to note

- All stories are wrapped with a [common decorator](https://storybook.js.org/docs/react/writing-stories/decorators#story-decorators) which wraps stories in both a `<ThemeProvider>` but also a `<Card>` with padding. Stories that depend on exact viewport dimensions can opt out with the `padding: 0` parameter.
- Every story is rendered as a smoke test in a real browser with the [Vitest addon](https://storybook.js.org/docs/writing-tests/integrations/vitest-addon) (`pnpm test:browser`), which also runs story `play` functions. Take care when renaming stories or ids that `play` functions rely on.

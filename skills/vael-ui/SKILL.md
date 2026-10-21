---
name: vael-ui
description: Use whenever writing, editing, reviewing or debugging Vue code that uses vael-ui ("vael-ui" or "vael-ui/vapor" imports, or vael-ui in package.json), even when the request just names a component (a dialog, drawer, toast, data table, select, date picker, drag-to-reorder list or board, progress ring or gauge, loader, color picker, inline edit, tour, tree) without saying vael-ui. Covers the exact current props/slots/events of all 68 components, setup (vdom vs Vapor builds, auto-import, Nuxt, SSR), theming with ConfigProvider, the ui prop and --ui-* CSS variables, the animation-agnostic contract (motionCss/forceMount/beforeClose), imperative composables (openDialog, confirmAction, toast, useSortableGroup, useTour), and breaking changes between versions. Check it before guessing a prop name or hand-building something vael-ui already ships.
license: MIT
metadata:
  author: Mini-Sylar
  version: 1.3.0
---

# vael-ui `vael-ui@0.5.0`

Vue 3 component library. 68 components across Setup, Actions, Forms & Inputs, Selection, Overlays, Navigation & Menus, Feedback, Data Display, Layout & Structure, and Gestures, plus 13 standalone composables.

**References:** [Component index](./references/docs/_INDEX.md) · [Composable index](./references/composables/_INDEX.md) · [Animation integration](./references/guides/animation-integration.md) · [Auto import](./references/guides/auto-import.md) · [Styling & CSS variables](./references/guides/styling-and-layers.md) · [Changelog](./references/guides/changelog.md)

Before writing code, check a component's own reference file for its exact current props/slots/events/exposed. The conventions below are stable, but the exact API surface is generated from the library's real types and evolves; if something here ever disagrees with a reference file, the reference file is right.

## What makes this library different

1. **Two compiled variants of every component**, from the same source: a normal Vue DOM (vdom) build and a Vue Vapor build.
2. **Animation-agnostic.** Every component that shows/hides content exposes real hooks (`motionCss`, `forceMount`, `beforeClose`, exposed element refs) so an app can swap the built-in CSS transition for GSAP, motion-v, or anything else. See [Animation integration](./references/guides/animation-integration.md) before reaching for CSS overrides or `!important` to fight a default transition.

## Install

```sh
npm install vael-ui vue   # or pnpm/yarn/bun
```

Requires Vue `^3.5.0`. The Vapor build (`vael-ui/vapor`) requires Vue `^3.6.0` and `createVaporApp`. Vapor is still pre-1.0 in Vue itself, so **pin an exact prerelease version** (e.g. `3.6.0-rc.12`, the version `vael-ui@0.5.0` is compiled against), not a floating `rc`/`beta` dist-tag, to avoid a compiler/runtime mismatch between what vael-ui's Vapor build was compiled against and what's actually installed. If Vapor components render with no styles, or throw errors specifically inside `<Transition>`-wrapped components, check this first.

A floating range such as `^3.5.0` can resolve to Vue 3.5, which has no Vapor at all. Install with `npm install vue@3.6.0-rc.12 --save-exact`.

A minimal Vapor app:

```ts
// main.ts
import { createVaporApp } from 'vue'
import App from './App.vue'

createVaporApp(App).mount('#app')
```

```vue
<!-- App.vue -->
<script setup vapor lang="ts">
import { Button, Toaster, toast } from 'vael-ui/vapor'
</script>

<template>
  <Toaster />
  <Button @click="toast('Saved')">Save</Button>
</template>
```

To compile every `<script setup>` SFC in the app as Vapor without the per-file `vapor` attribute, use `vue({ features: { vapor: true } })` (`@vitejs/plugin-vue` 6.0.8+). vael-ui never uses the Options API, so `optionsAPI: false` in `features` is safe for the library and drops that code from the bundle, as long as the app's own components don't use it.

Components set no font of their own (`font: inherit`), so set a `font-family` on `body`, or text renders in the browser's default serif.

Auto-import via `unplugin-vue-components` is the recommended setup instead of importing every component by hand. See [Auto import](./references/guides/auto-import.md).

### Server rendering

Both builds server-render and hydrate (Nuxt, Vite SSR, static sites). `vael-ui/vapor` resolves to an SSR build on the server automatically and hydrates as Vapor in the browser. For plain Vite SSR:

- Set `ssr: { noExternal: ['vael-ui'] }`. Every component imports its own CSS, which Node can't load if the package stays external.
- Overlays, Toaster and TooltipHost teleport to `body`, so inject `ctx.teleports` into the HTML template, or hydration mismatches:

  ```js
  const ctx = {}
  const appHtml = await renderToString(app, ctx)
  const html = template
    .replace('<!--app-html-->', appHtml)
    .replace('<!--teleports-->', ctx.teleports?.body ?? '') // <!--teleports--> sits right before </body>
  ```

### Nuxt

A Nuxt module ships as `vael-ui/nuxt`. Add it to `modules` in `nuxt.config.ts` and it registers every component (only components). Set `vaelUi: { variant: 'vapor' }` (Nuxt 4.6+, Vue 3.6 pinned) to use the Vapor build; the module turns on Nuxt's `vue.vapor` for you. Nuxt handles `noExternal` and teleports itself. You still:

- mount `<TooltipHost />`, `<DialogHost />`, `<PopoverHost />` and `<Toaster />` once in `app.vue`;
- import composables (`toast`, `openDialog`, `confirmAction`, ...) from `'vael-ui'` (or `'vael-ui/vapor'` with the Vapor variant);
- register directives in a plugin:

  ```ts
  // plugins/vael-ui.ts
  import { vTooltip, vScrollMask } from 'vael-ui'

  export default defineNuxtPlugin((nuxtApp) => {
    nuxtApp.vueApp.directive('tooltip', vTooltip)
    nuxtApp.vueApp.directive('scroll-mask', vScrollMask)
  })
  ```

## Upgrading and version notes

Code written against an older vael-ui (or remembered from one) can break on these. The [changelog](./references/guides/changelog.md) has every release in full.

- `0.5.0`: **List hover** in Select, Combobox, CommandPalette, Menu, Tree, MenuList, DataTable, Pagination, Tabs, SelectButton, Toolbar and Calendar is one sliding `.ui-indicator`, not a background on each row; a row `:hover` background you added paints over it, so set `--ui-indicator-bg` instead. **Button colours** come from `--ui-button-bg` / `--ui-button-border` on a `::before` layer; a `background` or `border` set on the button is now hidden under it (see [Button](./references/docs/Button.md)). **Sortable**'s root is a `.ui-sortable` div with the `<ul>` inside as `.ui-sortable-list`, so selectors that assumed the root was the list need updating. New: `Editable`, `ColorPicker`, Progress rings/gauges/segments, Loader `type`, Sortable `preview-mode="clone"` and `restrict-to-list`.
- `0.4.8`: with two months, a month picked from the second month's name lands second.
- `0.4.6`: form fields on touch screens no longer jump to 16px; call `preventFocusZoom()` once, or set `--ui-touch-font-size: 16px`.
- `0.4.0`: Select/Combobox/TreeSelect panels cap at `maxPanelHeight` 320; DatePicker opens on click/ArrowDown/Enter/Space, not on focus; popovers return focus to their trigger; `PasswordInput` sends `class`/`style` to its wrapper; `ConfigProvider` always renders its `display: contents` wrapper.
- `0.3.11`: Combobox `allowCustom` no longer commits on blur (`commit-on-blur` restores it).
- `0.3.4`: SwipeToReveal's `#actions` and `side` are gone; use `#leading-actions` / `#trailing-actions`.

## Best Practices

- **Pick one build for a new project.** `import { Button } from 'vael-ui'` (vdom, works with `createApp`) and `import { Button } from 'vael-ui/vapor'` (works with `createVaporApp`) export identical names/props/composables; only the compiled implementation differs. Using vdom imports with `createVaporApp` (or vice versa) without the interop plugin breaks.

  ```ts
  // vdom
  import { createApp } from 'vue'
  import { Button } from 'vael-ui'

  // vapor
  import { createVaporApp } from 'vue'
  import { Button } from 'vael-ui/vapor'
  ```

  Mixing is for migrating an existing vdom app to Vapor a few components at a time: keep `createApp`, add Vue's `vaporInteropPlugin`, and import from `'vael-ui/vapor'` inside the components marked `<script setup vapor>`.

  ```ts
  import { createApp, vaporInteropPlugin } from 'vue'
  import App from './App.vue'

  createApp(App).use(vaporInteropPlugin).mount('#app')
  ```

- **Don't fight the default transition with CSS.** If a component isn't animating the way an app needs, reach for `motionCss={false}` + `forceMount` + `beforeClose` (see [Animation integration](./references/guides/animation-integration.md)) before trying to override built-in transition timing via selector specificity.

  ```vue
  <Dialog ref="dialogRef" v-model:open="open" :before-close="beforeClose">
  ```

- **Theme app-wide via `ConfigProvider`, override per-instance via the `ui` prop.** Every component accepts a `ui` prop shaped like its internal part names; a part's value is either a plain class string or `{ class, style }` for cases a class alone can't express.

  ```vue
  <ConfigProvider :theme="{ primary: '#6d28d9', radius: '8px' }" size="sm">
    <App />
  </ConfigProvider>

  <Dialog :ui="{ panel: 'my-custom-panel-class' }" />
  ```

  `size` and `motionCss` on `ConfigProvider` are app-wide defaults; set them once there instead of on every component. Pass `i18n` (your `useI18n()`) and its locale formats dates and numbers too.

  `ui` part classes concatenate with internal classes by default (no dedup). Inject a real class merger (e.g. `tailwind-merge`'s `twMerge`) via `ConfigProvider` if the project uses Tailwind and needs conflicting utilities to resolve correctly.

- **Tune motion app-wide on `ConfigProvider`, not per component.** `indicator` sets how every sliding indicator moves (hover highlights and Tabs, SelectButton, MenuList selections): `'glide'` (default), `'instant'`, or a spring `{ damping, response }` where damping below 1 overshoots. `motion-css="false"` makes everything instant. `loader-type` sets every spinner, and takes your own component or a render function (`() => h(MySpinner, props)`) as well as `'ring' | 'dots' | 'bars' | 'comet' | 'dither'`. For the timing alone, set `--ui-duration-indicator` / `--ui-duration-indicator-wide` / `--ui-ease-indicator`.

  ```vue
  <ConfigProvider :indicator="{ damping: 0.55, response: 0.3 }" :loader-type="MySpinner">
    <App />
  </ConfigProvider>
  ```

- **Give your own lists the same sliding highlight with `useHoverIndicator`**, and build custom spring motion with `createSpring` / `useSpringValue` (the engine the components use). See the [composable index](./references/composables/_INDEX.md), which also lists the headless engines behind components (`useTabs`, `useMenu`, `useListbox`, `useSlider`, …).

- **Check the [component index](./references/docs/_INDEX.md) before building UI by hand.** Gauges and donut charts (`Progress type="circle"`), stacked usage bars (`Progress` with segments), spinners (`Loader`, or `Button` `loading`), inline rename (`Editable`), colour pickers, boards (`Sortable` + `useSortableGroup`), tours, OTP fields, swipe actions and pull-to-refresh all ship. A hand-drawn SVG or a custom drag handler usually means a component was missed.

- **Prefer imperative composables over manual `v-model` juggling for one-off flows.** `confirmAction()`, `openDialog()`/`openPopover()`, and `toast()` all return real Promises/imperative handles. See the [composable index](./references/composables/_INDEX.md).

  ```ts
  import { confirmAction } from 'vael-ui'

  const { result } = confirmAction({
    title: 'Delete this item?',
    variant: 'danger',
    onConfirm: async () => api.delete(id),
  })
  if (await result) refresh()
  ```

- **Mind cascade layers when overriding styles.** vael-ui's CSS lives in `@layer ui-components`; an app's own *unlayered* CSS already wins by default (correct for one-off overrides), but an app's own *layered* base styles (some CSS resets are layered) need to be ordered before `ui-components` explicitly, or they'll silently lose regardless of specificity. See [Styling & CSS variables](./references/guides/styling-and-layers.md).

- **Reach for CSS variables before reaching for `ui` prop overrides on every instance.** Every color/radius/shadow/motion value is a `--ui-*` custom property. For a global change (brand color, corner radius), override the variable once instead of passing `ui` to every component instance.

- **Let components own toggle state instead of wiring `aria-pressed` by hand.** A lone toggle is `<Button v-model:pressed="bold">`; a row of toggles is a `ButtonGroup` with a `v-model` and a `value` on each `Button` (`multiple` for several on at once, `:allow-empty="false"` to keep one on). A ghost/outline/secondary toggle already looks pressed when on. For a custom on-state look, bind a class from the same value; `[aria-pressed='true']` (or Tailwind's `aria-pressed:` in `ui.root`) styles every toggle at once.

  ```vue
  <Button v-model:pressed="starred" variant="ghost" icon aria-label="Star" :class="{ starred }">
    <StarIcon />
  </Button>
  ```

- **Colour a Button through `--ui-button-bg` / `--ui-button-border`.** The box is a `::before` layer, so `background`/`border` on the element itself disappear under it. Text colour is plain `color`; a dashed border is `.my-button::before { border-style: dashed }`. For every primary button at once, change `--ui-primary` instead.

- **Group options with `{ label, items }` entries, not separate components.** `Select` and `Combobox` take them mixed with plain options; customise the heading with `#group-label` (it receives your group object, so extra fields like `icon` or `color` just work) and style with `ui.group` / `ui.groupLabel`. Making labels sticky? Add `scroll-padding-block-start` on `ui.list` so keyboard scrolling doesn't park the highlight under the label. Collapsible sections are a tree: use `TreeSelect`.

- **Give a component `min-inline-size: 0` when it sits inside a shrinkable flex row/column** (a resizable panel, a narrow sidebar). A flex item's default `min-width: auto` refuses to shrink below its content's natural width — long text, an inline `<input>`, anything not already ellipsis-truncated — so without it the component (or the row causing it) overflows its container instead of shrinking or truncating to fit.

  ```css
  .my-resizable-sidebar > * {
    min-inline-size: 0;
  }
  ```

- **Give a component `flex: 1` (not the flex default) when its own responsive behavior depends on measuring its available width** — `Toolbar`'s overflow-collapse is the concrete case: it watches its own root via `ResizeObserver`. Left at the flex default (`flex: 0 1 auto`), the root only ever sizes to fit its *current* content, so once something collapses into the overflow menu, the observer never sees that more room exists and previously-collapsed items never restore after the container grows back.

## Common mistakes

- Importing from `vael-ui` while using `createVaporApp` (or the reverse) without `vaporInteropPlugin`.
- Using `resolve: { conditions: ['vapor'] }` alone. It replaces Vite's default conditions; spread `defaultClientConditions` / `defaultServerConditions` (see [Auto import](./references/guides/auto-import.md#one-shared-import-no-vapor-subpath)).
- Registering `v-tooltip` without mounting `<TooltipHost />`: the tooltip never shows and nothing warns.
- Using a floating `vue@rc`/`vue@beta` dist-tag with Vapor instead of an exact pinned prerelease version. Causes compiler/runtime mismatches that surface as confusing runtime errors, especially around `<Transition>`.
- Styling a list row's `:hover` background in a vael-ui list (Menu, Select, Tree, DataTable, …): it paints over the sliding highlight. Set `--ui-indicator-bg`.
- Setting `background` or `border` on a Button (they vanish under its `::before` layer since 0.5.0); use `--ui-button-bg` / `--ui-button-border`.
- Adding `:aria-pressed` and a click handler to make a toggle; `v-model:pressed` does both.
- Assuming `ui` prop values dedupe against internal classes automatically. They don't unless a class merger is configured.
- Reaching for `vael-ui/style.css` (the full, untreeshaken bundle) by default. Per-component CSS already loads automatically via each component's own JS import; only use the full bundle if there's a specific reason to.
- Embedding a component in a resizable/narrow flex layout without `min-inline-size: 0` — it overflows the container instead of shrinking.
- Leaving `Toolbar` (or anything else with its own `ResizeObserver`-driven behavior) at the flex default instead of `flex: 1` — its own size never reflects real available space.

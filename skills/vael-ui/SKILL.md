---
name: vael-ui
description: Use whenever writing, editing, or debugging Vue 3 code that imports from "vael-ui" or "vael-ui/vapor", an animation-agnostic component library with full Vue Vapor support. Covers installation, the vdom/vapor split, auto-import, theming, the animation-agnostic contract (motionCss/forceMount/beforeClose), and imperative composables (openDialog, openPopover, confirmAction, useToast, useTour).
license: MIT
metadata:
  author: Mini-Sylar
  version: 1.0.2
---

# vael-ui `vael-ui@0.4.6`

Vue 3 component library. 66 components across Setup, Actions, Forms & Inputs, Selection, Overlays, Navigation & Menus, Feedback, Data Display, Layout & Structure, and Gestures, plus 13 standalone composables.

**References:** [Component index](./references/docs/_INDEX.md) · [Composable index](./references/composables/_INDEX.md) · [Animation integration](./references/guides/animation-integration.md) · [Auto import](./references/guides/auto-import.md) · [Styling & CSS variables](./references/guides/styling-and-layers.md)

Before writing code, check a component's own reference file for its exact current props/slots/events/exposed. The conventions below are stable, but the exact API surface is generated from the library's real types and evolves; if something here ever disagrees with a reference file, the reference file is right.

## What makes this library different

1. **Two compiled variants of every component**, from the same source: a normal Vue DOM (vdom) build and a Vue Vapor build.
2. **Animation-agnostic.** Every component that shows/hides content exposes real hooks (`motionCss`, `forceMount`, `beforeClose`, exposed element refs) so an app can swap the built-in CSS transition for GSAP, motion-v, or anything else. See [Animation integration](./references/guides/animation-integration.md) before reaching for CSS overrides or `!important` to fight a default transition.

## Install

```sh
npm install vael-ui vue   # or pnpm/yarn/bun
```

Requires Vue `^3.5.0`. The Vapor build (`vael-ui/vapor`) requires Vue `^3.6.0` and `createVaporApp`. Vapor is still pre-1.0 in Vue itself, so **pin an exact prerelease version** (e.g. `3.6.0-rc.12`, the version `vael-ui@0.4.6` is compiled against), not a floating `rc`/`beta` dist-tag, to avoid a compiler/runtime mismatch between what vael-ui's Vapor build was compiled against and what's actually installed. If Vapor components render with no styles, or throw errors specifically inside `<Transition>`-wrapped components, check this first.

The Vapor build also server-renders (Nuxt, Vite SSR, static sites): server runtimes get an SSR build of the same components automatically, and the browser hydrates it as Vapor, so the same `vael-ui/vapor` import works everywhere.

A Nuxt module ships as `vael-ui/nuxt`. Add it to `modules` in `nuxt.config.ts` instead of manual imports. Set `vaelUi: { variant: 'vapor' }` (Nuxt 4.6+) to use the Vapor build; the module turns on Nuxt's `vue.vapor` for you.

Auto-import via `unplugin-vue-components` is the recommended setup instead of importing every component by hand. See [Auto import](./references/guides/auto-import.md).

## API Changes

- `0.4.6`: **New on `ConfigProvider`:** `size` (`'sm' | 'md' | 'lg'`) sets the default size of every button, form control and table inside, and `:motion-css="false"` makes everything inside change instantly (panels still open and close, toasts and list rows still leave). A component's own `size` / `motionCss` wins, and a component's own `motion-css="false"` still hands animation to its events. Pass `useI18n()` as `i18n` and its current `locale` formats `Calendar`, `DatePicker` and `InputNumber`; vue-i18n no longer warns about `uiKit` keys you haven't translated, and an app that translated some but not all gets one dev-only warning per locale listing the missing ones. `theme.fontSize` sets component text size. **Typography:** all component text, form fields included, follows your `html` font size. **New `preventFocusZoom()`:** call it once in `main.ts` so iPhones don't zoom into fields under 16px. **Behaviour change:** form fields on touch screens no longer jump to 16px (`--ui-touch-font-size` now defaults to `0px`); call `preventFocusZoom()`, or set `--ui-touch-font-size: 16px` for the old behaviour. **Fix:** `InputNumber` redraws when `locale`, `currency` or `mode` changes. **New in the animation hand-off:** Dialog, Drawer and BottomSheet expose the backdrop as `overlayEl` (with `forceMount`, an inline `opacity` replaces the built-in fade). BottomSheet takes `container` and `teleportTo` like Drawer, and exposes `releaseVelocity` for continuing a flick in `beforeClose`; when your `beforeClose` already moved the sheet off screen, `done()` closes it at once. Stepper exposes `circleEls`/`connectorEls` and its fill reads `--ui-stepper-fill`. SpeedDial exposes `iconEl`. Tour exposes `spotlightEl`/`positionerEl` and takes `motionCss` (`false` turns off the spotlight glide). Stepper's and SpeedDial's `motion-css="false"` now covers every built-in transition. **Toaster:** `<Toaster name="editor">` plus `toast('…', { toaster: 'editor' })` targets one Toaster; untargeted toasts go to the unnamed one. **Drawer** now passes `flush` and `maximizable` through. A ConfigProvider's `motion-css="false"` also reaches teleported overlays.
- `0.4.5`: **Vapor:** `vael-ui/vapor` now server-renders and hydrates as Vapor. **Nuxt:** `vaelUi: { variant: 'vapor' }` (Nuxt 4.6+) registers `vael-ui/vapor` and turns on `vue.vapor`; add `vite: { vue: { features: { vapor: true } } }` to compile your own `<script setup>` as Vapor too (`features.vapor` in `@vitejs/plugin-vue` 6.0.8+ does the same outside Nuxt). **`Calendar` / `DatePicker`:** `months` (e.g. `:months="2"`) shows months side by side and stacks them on narrow screens; new `#label` slot for the month and year heading; `DatePicker` gets the same `#day` slot as `Calendar` and a `closeOnSelect` prop (`false` keeps the panel open after picking). Switching between days, months and years now zooms, and there is no dead space between days. **`Accordion`:** `v-model:value` and `@change` take the type you bind (`'a' | 'b'`, or an array of it with `multiple`). **`CommandPalette`:** the results list eases to its height as you type; `motionCss: false` hands rows to `@item-enter` / `@item-leave`. **`Slider`:** `#thumb` and `#track` slots. **`OtpInput`:** `groups` with a `#separator`. **`DataTable`:** edges fade while more can scroll (`scrollFade: false` turns it off). **`Timeline`:** `@item-enter` / `@item-leave` receive a third `parts` argument with the step's elements. **New `motionCss` on `Radio`, `Pagination` and `PullToRefresh`**, with exposed `controlEl`/`dotEl`, `indicatorEl`/`labelsEl`/`listEl` and `zoneEl`; `Dock` exposes `itemEls()` (use `:magnify="false"` to turn its magnification off). **Fixes:** registering a component globally under its own name (`app.component('Button', Button)`) no longer recurses, and `nativeTag()` is exported for your own `as`-prop components. On touch screens form controls use at least 16px text (`--ui-touch-font-size`, `0px` turns it off).
- `0.4.4`: **New on `Toaster`:** `expand` keeps the stack open as a list; timers keep running, and hover still pauses them. **New `toast()` option:** `pinned: true` shows that toast in its own section beside the stack, where newer toasts can't cover it, and it doesn't count toward `maxVisible`. Pair it with `duration: Infinity` for a notice that stays until someone closes it. **New `toast()` option:** `id: 'verify-email'` updates that toast in place when shown again, and `toast.dismiss('verify-email')` closes it, so you don't need to store the returned number. `ToastEntry.id` is now `number | string` (`ToastId`). Toasts queued past `maxVisible` now keep their full time until they show.
- `0.4.3`: **New on `Field`:** `labelPlacement` takes `'start'`/`'end'` for labels beside the control, with `labelWidth` to line up a column of fields and `labelAlign` for the text. `attached` joins a side label to the control as one box. `#prepend`/`#append` add cells to any control: text and icons share a segment, and each control in a cell (a Select, a Button) gets its own, divided. Give controls in a cell an `aria-label`, since the Field's label doesn't name them. Cell colors come from `--ui-field-cell-bg`/`--ui-field-cell-color`.
- `0.4.0`: **Behaviour changes:** `Select`, `Combobox` and `TreeSelect` panels now cap at `maxPanelHeight` 320 by default (`Infinity` restores full height). `DatePicker` opens on click, ArrowDown, Enter or Space, no longer on focus. Popovers, menus and pickers return focus to their trigger when they close. `PasswordInput` routes `class`/`style` to its wrapper and other attributes to the input. With `motionCss: false`, `Collapsible` and `Accordion` write no inline height, and closed panels are `inert`. `ConfigProvider` always renders its `display: contents` wrapper. **New:** `Dock` `grow` (magnified items change real size and the dock grows with them; the transform-based magnification stays the default), `Stepper` `#indicator` slot, `Tabs` `idBase` plus `panelProps(value)` for wiring `role="tabpanel"`, `Menu` `openPath`, keyboard row selection in `DataTable` `selectionMode="row"`, and the `--ui-danger-solid` token for solid danger fills.
- `0.3.11`: **`Combobox`:** with `allowCustom`, clicking away no longer commits the typed text; set `commit-on-blur` for the old behavior. A `Create "…"` row now offers the typed text (customize with `#create`, hide with `:create-option="false"`), `@create` receives `{ reason, cancel }`, and the new `tab-behavior` prop (`'select'` or `'create'`) decides what Tab does.
- `0.3.8`: **`Calendar`** has a `#day` scoped slot for custom day content. **`Tree`** has a `forceMount` prop for driving the collapse animation yourself.
- `0.3.7`: **`Popover`** has an opt-in `openOnTriggerClick` prop: click-to-toggle and automatic trigger positioning from `#trigger`, the same contract `Menu` has.
- `0.3.5`: **`Toaster`** and **`DataTable`** take a `ui` prop and a `theme.toaster` / `theme.dataTable` slice, like every other component.
- `0.3.4` — **`SwipeToReveal`:** actions now go in `#leading-actions` / `#trailing-actions` (one slot for a single-edge row, both for two). The generic `#actions` slot and the `side` prop are removed — migrate `#actions` → `#trailing-actions`, and `side="leading"` + `#actions` → `#leading-actions`. New: `progress` slot prop / `leadingProgress` · `trailingProgress` exposed / `--ui-swipe-reveal-progress` CSS var (0 → 1 per edge), and a `revealScale` grow-in that's on by default (`:reveal-scale="false"` to opt out).
- `0.2.x`: added a `vapor` package export condition, letting a whole project resolve bare `import { X } from 'vael-ui'` to the Vapor build via `resolve.conditions`/`customConditions`, instead of importing from the `/vapor` subpath everywhere. See [Auto import](./references/guides/auto-import.md#one-shared-import-no-vapor-subpath).
- Pre-`0.2.0`: CSS wasn't split per component. Only relevant if pinned to an old version.

## Best Practices

- **Pick vdom or vapor per-project, don't mix them.** `import { Button } from 'vael-ui'` (vdom, works with `createApp`) and `import { Button } from 'vael-ui/vapor'` (works with `createVaporApp`) export identical names/props/composables; only the compiled implementation differs. Using vdom imports with `createVaporApp` (or vice versa) breaks.

  ```ts
  // vdom
  import { createApp } from 'vue'
  import { Button } from 'vael-ui'

  // vapor
  import { createVaporApp } from 'vue'
  import { Button } from 'vael-ui/vapor'
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

- **Give a component `min-inline-size: 0` when it sits inside a shrinkable flex row/column** (a resizable panel, a narrow sidebar). A flex item's default `min-width: auto` refuses to shrink below its content's natural width — long text, an inline `<input>`, anything not already ellipsis-truncated — so without it the component (or the row causing it) overflows its container instead of shrinking or truncating to fit.

  ```css
  .my-resizable-sidebar > * {
    min-inline-size: 0;
  }
  ```

- **Give a component `flex: 1` (not the flex default) when its own responsive behavior depends on measuring its available width** — `Toolbar`'s overflow-collapse is the concrete case: it watches its own root via `ResizeObserver`. Left at the flex default (`flex: 0 1 auto`), the root only ever sizes to fit its *current* content, so once something collapses into the overflow menu, the observer never sees that more room exists and previously-collapsed items never restore after the container grows back.

## Common mistakes

- Importing from `vael-ui` while using `createVaporApp` (or the reverse).
- Using a floating `vue@rc`/`vue@beta` dist-tag with Vapor instead of an exact pinned prerelease version. Causes compiler/runtime mismatches that surface as confusing runtime errors, especially around `<Transition>`.
- Assuming `ui` prop values dedupe against internal classes automatically. They don't unless a class merger is configured.
- Reaching for `vael-ui/style.css` (the full, untreeshaken bundle) by default. Per-component CSS already loads automatically via each component's own JS import; only use the full bundle if there's a specific reason to.
- Embedding a component in a resizable/narrow flex layout without `min-inline-size: 0` — it overflows the container instead of shrinking.
- Leaving `Toolbar` (or anything else with its own `ResizeObserver`-driven behavior) at the flex default instead of `flex: 1` — its own size never reflects real available space.

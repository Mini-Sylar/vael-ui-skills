---
name: vael-ui
description: Use whenever writing, editing, reviewing or debugging Vue code that uses vael-ui ("vael-ui" or "vael-ui/vapor" imports, or vael-ui in package.json), even when the request only names a component (dialog, drawer, toast, data table, select, date picker, sortable board, tree) or asks to animate one. Covers the exact props, slots, events and motion parts of all 70 components; setup (vdom vs Vapor builds, auto-import, Nuxt, SSR); theming with ConfigProvider, the ui prop and --ui-* variables; the motion system (the `motion` prop for off, springs, part values, states, lists, `morph` from the trigger; `v-animate` with layout, `layoutId` and `.size`; enter/leave hooks for GSAP or motion; ConfigProvider motion styles and presets; useMotion; settled() in tests) with rules for motion that feels right; imperative composables (openDialog, confirmAction, toast); and upgrading from 0.4. Check it before guessing a prop name, hand-building something vael-ui ships, or animating around a vael-ui component.
license: MIT
metadata:
  author: Mini-Sylar
  version: 2.0.0
---

# vael-ui `vael-ui@0.5.0`

A Vue 3 component library: 70 components, from buttons and form fields to overlays, data tables, drag-and-drop and gestures, plus standalone and headless composables.

**References:** [Components](./references/docs/_INDEX.md) · [Composables](./references/composables/_INDEX.md) · [Motion](./references/guides/motion.md) · [Auto import](./references/guides/auto-import.md) · [Styling & CSS variables](./references/guides/styling-and-layers.md) · [Changelog](./references/guides/changelog.md)

Check a component's reference file before writing code. It lists the exact props, slots, events, exposed values and motion parts, generated from the library's types. If this page ever disagrees with a reference file, the reference file is right.

## What makes vael-ui different

1. **Two builds of every component** from one source: a regular Vue (vdom) build, `vael-ui`, and a Vue Vapor build, `vael-ui/vapor`. Same names, props and composables.
2. **One motion system you can take over part by part.** Every component moves well with nothing set. One `motion` prop, the same shape everywhere, lets you go as far as you need: turn it off, retime it with a spring, change one part's values, animate a state, hand the enter and exit to GSAP or motion, or swap the engine for the whole app. Your own elements join in with `v-animate`. Read the [Motion guide](./references/guides/motion.md) before writing any animation near a vael-ui component.

## Install and set up

```sh
npm install vael-ui vue
```

Requires Vue `^3.5.0`. The Vapor build needs Vue `^3.6.0` and `createVaporApp`. Vapor is pre-1.0 in Vue, so **pin an exact prerelease** (`npm install vue@3.6.0-rc.13 --save-exact`, the version `vael-ui@0.5.0` is compiled against). A floating `rc` tag causes compiler/runtime mismatches, often inside `<Transition>`; a range like `^3.5.0` can resolve to 3.5, which has no Vapor.

```ts
// main.ts (Vapor)
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

- **Auto-import** with `unplugin-vue-components` and `VaelUiResolver` from `vael-ui/resolver` instead of importing every component. See [Auto import](./references/guides/auto-import.md).
- **Whole app as Vapor:** `vue({ features: { vapor: true } })` (`@vitejs/plugin-vue` 6.0.8+). `optionsAPI: false` is safe for vael-ui. vue-tsc only types an SFC as Vapor when it has the `vapor` attribute, so without it cast the root: `createVaporApp(App as unknown as VaporComponent)`. With the `vapor` export condition, add `"customConditions": ["vapor"]` to `tsconfig` so `vael-ui` types resolve the same way.
- **Fonts:** components use `font: inherit`, so set a `font-family` on `body`.
- **Mount the hosts once:** `<Toaster />`, and `<TooltipHost />`, `<DialogHost />`, `<PopoverHost />` if you use `v-tooltip`, `openDialog()` or `openPopover()`. With a `ConfigProvider`, mount them inside it: what a host shows (`toast()`, `openDialog()`, tooltips) takes the provider's theme, motion, messages and size only from a provider above the host.
- **Directives** aren't auto-imported. Import `vTooltip`, `vAnimate`, `vScrollMask` or `vHoverIndicator` in `<script setup>`, or register them with `app.directive('tooltip', vTooltip)`.

**Server rendering.** Both builds server-render and hydrate. For plain Vite SSR, set `ssr: { noExternal: ['vael-ui'] }` (each component imports its own CSS) and inject `ctx.teleports?.body` before `</body>`, since overlays teleport to `body`. `v-animate` entrances are written as CSS on the server and never replay on hydration.

**Nuxt.** Add `vael-ui/nuxt` to `modules`. It registers every component and `v-animate`. `vaelUi: { variant: 'vapor' }` (Nuxt 4.6+, Vue 3.6 pinned) switches to the Vapor build. Still mount the hosts in `app.vue`, import composables from `'vael-ui'`, and register `v-tooltip` in a plugin:

```ts
// plugins/vael-ui.ts
import { vTooltip } from 'vael-ui'

export default defineNuxtPlugin((nuxtApp) => {
  nuxtApp.vueApp.directive('tooltip', vTooltip)
})
```

## Conventions agents get wrong

- **One build per app.** `vael-ui` goes with `createApp`, `vael-ui/vapor` with `createVaporApp`. Mixing them needs Vue's `vaporInteropPlugin`, and is for migrating a vdom app a few components at a time.
- **Don't fight built-in motion with CSS.** No `!important` transitions, no `transition` on a part vael-ui animates, no `<Transition>` or motion-v `<AnimatePresence>` wrapped around a vael-ui panel. Use `:motion` (see [Motion](#motion)).
- **`before-close` is a guard, not an animation.** Return `true` to close, `false` to stay open, or a promise: `:before-close="() => confirmAction({ title: 'Discard changes?' }).result"`. Animate a close with `motion.exit` or a `leave` hook.
- **`force-mount` keeps content mounted** so its state survives a close. It doesn't hand you the animation.
- **Your own lists get the sliding hover highlight with `v-hover-indicator`,** not a `:hover` background on each row: `<ul v-hover-indicator="'.row'">`. The value is an expression, so the selector needs inner quotes. Rows with a solid background (SwipeToReveal rows, cards) take `.over`, which draws the highlight as a tint on top. The highlight is the list's first child, so a `:first-child` rule on the rows needs `:first-of-type` or a class. On a panel whose background is `--ui-muted`, set `--ui-indicator-bg: var(--ui-muted-hover)`, or the highlight matches the panel.
- **Style a built-in chevron, clear button or icon through its `ui` key** (`chevron`, `clear`, `icon`, `filterIcon`, …) or a `ConfigProvider` theme, not a deep selector on its class.
- **Theme once, override per instance.** `ConfigProvider`'s `theme`, `size`, `motion`, `indicator`, `loader-type`, `haptics` and `i18n` are app-wide defaults. For a global change, set a `--ui-*` variable once. For one instance, use the `ui` prop: part names map to a class or `{ class, style }`.

  ```vue
  <ConfigProvider :theme="{ primary: '#6d28d9', radius: '8px' }" size="sm">
    <App />
  </ConfigProvider>

  <Dialog :ui="{ panel: 'my-panel' }" />
  ```

- **`ui` classes don't dedupe.** With Tailwind, pass `twMerge` as `ConfigProvider`'s `class-merge` so conflicting utilities resolve.
- **Mind cascade layers.** vael-ui's CSS is in `@layer ui-components`. Your unlayered CSS wins; your layered resets must be ordered before `ui-components`. See [Styling](./references/guides/styling-and-layers.md).
- **Color a Button with `--ui-button-bg` / `--ui-button-border`.** The box is a `::before` layer, so `background` and `border` on the element vanish under it. Dashed: `.my-button::before { border-style: dashed }`.
- **Restyle list hover with `--ui-indicator-bg`.** Select, Menu, Tree, DataTable, Tabs and the other lists slide one `.ui-indicator` behind their rows. A row `:hover` background paints over it.
- **Toggles own their state.** One toggle: `<Button v-model:pressed="bold">`. A row: `ButtonGroup` with `v-model` and a `value` on each `Button` (`multiple`, `:allow-empty="false"`). Don't wire `aria-pressed` by hand.
- **Group options with `{ label, items }` entries** in `Select` and `Combobox` items. Style with `ui.group` / `ui.groupLabel`; customize with `#group-label`. Collapsible groups are a tree: use `TreeSelect`.
- **Prefer imperative composables for one-off flows.** `confirmAction()`, `openDialog()`, `openPopover()` and `toast()` return promises or handles.

  ```ts
  const { result } = confirmAction({ title: 'Delete this item?', variant: 'danger', onConfirm: () => api.delete(id) })
  if (await result) refresh()
  ```

- **Check the [component index](./references/docs/_INDEX.md) before building by hand.** Gauges and donuts (`Progress type="circle"`), stacked bars (`Progress` segments), spinners (`Loader`), inline rename (`Editable`), color pickers, rolling numbers (`AnimatedNumber`), text swaps (`AnimatedText`), boards (`Sortable` + `useSortableGroup`), tours, OTP fields, swipe actions, pull-to-refresh and hold-to-confirm (`Button hold`) all ship.
- **Shrinkable flex layouts:** give a component `min-inline-size: 0` inside a resizable or narrow flex container, or it overflows instead of shrinking. Give `Toolbar` `flex: 1` so its overflow menu can measure the room it has.
- **Don't import `vael-ui/style.css` by default.** Each component loads its own CSS.
- **vue-tsc doesn't narrow `v-if` inside a directive's value.** `` v-if="selected" v-animate="{ layoutId: `site-${selected!.id}` }" `` needs the `!`; at runtime the value is never read again once the `v-if` drops it.
- **Combobox and Select options hold no buttons.** An option's content is flattened for screen readers and focus stays in the field, so a delete button inside `#item` works only with a mouse. Put removal outside the list.
- **Size a dropdown's panel with `panel-width`,** not `ui.panel` styles: `'trigger'` follows the field, a number is px, a string is any CSS length (`panel-width="20rem"`). Select, Combobox, TreeSelect, CascadeSelect, DatePicker and Menu take it; on a phone it still fits the screen. In CascadeSelect it sizes the first level only, and unset it's the field's width up to 16rem, so the next level opens close by; don't stretch it to a long field. A wider DatePicker gets bigger days.
- **v-draggable and `useSortable` reorder a wrapping grid as is:** items that wrap onto more lines (a card grid) make room slot by slot in reading order, on either `axis`. Don't build a grid drag by hand. Cards wrapped in a ContextMenu or Tooltip trigger drag fine, and cards that also carry `v-animate="{ layout: 'position' }"` move once on a drop.
- **Pull to refresh on a scrolling page:** wrap the content in `PullToRefresh` with `:scroll-el="document.documentElement"` and the same handler as the visible refresh button, which keeps working for keyboard and desktop. Mouse drags pull too, so mount it only where touch is the input (a phone layout).
- **`resolve.conditions: ['vapor']` alone breaks Vite.** Spread `defaultClientConditions` / `defaultServerConditions` (see [Auto import](./references/guides/auto-import.md#one-shared-import-no-vapor-subpath)).

## Motion

The full guide is [references/guides/motion.md](./references/guides/motion.md): the API by task, then rules for motion that feels right, then common mistakes. The short version:

```vue
<!-- Off -->
<Dialog v-model:open="open" :motion="false" />

<!-- Same look, different feel -->
<Drawer v-model:open="open" :motion="{ spring: 'gentle' }" />

<!-- Values by part (the part names are the component's ui keys) -->
<Dialog v-model:open="open" :motion="{ initial: { panel: { opacity: 0, y: 16, scale: '0.96' } } }" />

<!-- A state -->
<Input v-model="email" :invalid="invalid" :motion="{ states: { invalid: { root: { x: [0, -6, 6, -3, 0] } } } }" />

<!-- Grow out of the button that opened it -->
<Popover v-model:open="open" open-on-trigger-click :motion="{ morph: 'trigger' }">…</Popover>

<!-- Your own elements: modifiers pick (an entrance, a spring, stagger), the value tunes -->
<li v-for="task in tasks" :key="task.id" v-animate:tasks.rise.stagger="{ layout: true }">…</li>
<span v-animate.pop.bouncy>New</span>

<!-- Hover and press on any element, as on a component -->
<div v-animate="{ states: { hover: { scale: 1.03 }, active: { scale: 0.97 } } }">…</div>

<!-- Pages: the old one leaves, then the new one comes in -->
<RouterView v-slot="{ Component, route }">
  <component :is="Component" :key="route.path" v-animate:page="{ initial: { opacity: 0, y: 8 }, swap: 'wait' }" />
</RouterView>

<!-- The whole app on one spring -->
<ConfigProvider :motion="{ spring: 'snappy' }"><App /></ConfigProvider>
```

```ts
// Your own library: return the animation; vael-ui waits for it and stops it if interrupted
const motion: DialogMotion = {
  enter: ({ panel }) => gsap.from(panel, { opacity: 0, y: 16, duration: 0.3, ease: 'power3.out' }),
  leave: ({ panel }) => gsap.to(panel, { opacity: 0, duration: 0.12 }),
}
```

**Imports.** Components and their motion types (`DialogMotion`, `SelectPart`) come from `vael-ui`. Everything for your own elements comes from `vael-ui/motion`, in both builds: `vAnimate`, `animate`, `useAnimate`, `useMotion`, `useScrollProgress`, `useTween`, `settled`, `springEasing`, `useSpringValue` and the general types (`MotionValues`, `MotionTiming`, `MotionStyle`). In a `<script setup vapor>` component use `import { vAnimateVapor as vAnimate } from 'vael-ui/motion'` (the VDOM `vAnimate` is skipped there); with the `vapor` export condition, plain `vAnimate` is already the Vapor form.

**Names.** `v-animate:rows` names an element. A lone element needs no name. Elements under one name that come in together stagger as one group, inside a component they're its `targets.rows`, `after: 'rows'` waits for them, and pages under one name can `swap: 'wait'`.

Rules to apply every time:

- **Decide before you animate, in order:** how often people see it (hundreds of times a day or from the keyboard: no motion, and keep the built-in look on command palettes, menus and tabs), what it's for (feedback, where it came from, what changed, smoothing a jump; delight only for rare moments), then the tool (the component's `motion` prop, `v-animate`, `animate()`, `useTween`, hooks). Stop at "don't animate".
- **Pick values by what moves, not the nearest number.** A tooltip opens in about 125 ms from `scale: '0.98'`, a menu in 150 to 250 ms from `0.97` and 6 to 8 px, a dialog in 200 to 250 ms from `0.96` and 12 to 16 px; each closes faster. The full table: [Values by what moves](./references/guides/motion.md#5-durations-easing-and-springs).
- **Ease out both ways, leave faster than you enter**, and keep UI motion under 300 ms. Use the tokens by name: `easing: 'out'`, `duration: 'enter'`.
- **Springs for movement and anything people can grab**, starting at `'snappy'` or `'smooth'` (no overshoot). Bounce only after a flick or for a rare flourish; never on a close.
- **Never scale from 0.** Start at `scale: '0.95'` or higher with `opacity: 0`, and move 4 to 16 px.
- **Delay only to stagger, to filter an accidental trigger, or to sequence (`after`).** Something that feels late needs a shorter duration, not a delay, and a close is never delayed. Keep a stagger's total under about 300 ms.
- **Number on screen:** `useTween(() => total)` eases it to each new value; don't animate a number with a `setInterval`.
- **`layout` follows an element's place in its parent.** When something opens above a list (a detail panel), the list's box is what moves: give it `v-animate="{ layout: 'position' }"` too, or it snaps down while its items slide.
- **Animate `x`, `y`, `scale`, `rotate` and `opacity`.** No `width`, `height` or `top` in values; use `v-animate.size`, `resize` or `layout`.
- **Let reduced motion work.** vael-ui keeps fades and drops movement on its own, and skips hooks unless `reducedMotion: 'run'`.
- **In tests,** `await settled()` or wrap in `<ConfigProvider :motion="false">`.
- **Motion an event starts** (a wrong code shakes the field, a badge bumps on add): `animate(el, values, options)`, or `useAnimate()` inside a component; it follows the app's spring and reduced motion, and `await`s. Motion that follows state stays declarative: `states`, `v-animate`'s `animate` or `replay`.
- **Apply an overlay's result after it has left,** in its `onAfterLeave`: add the row, toast, navigate. Changed while the dialog fades, the page moves under it, the quick exit reads as a cut, and the new row's entrance plays hidden behind it.
- **Tables:** with your own row `motion`, a page change (no row kept) drops the old rows at once and brings the new ones in with your `initial`; `exit` plays when rows are removed or filtered out. Without it, a page change shows at once.
- **Acting once something has played**: put the callback on what animates. `onAfterEnter` / `onAfterLeave` go on an overlay's or list's `motion` (a list's run per item with `{ index }`), on a state (`states.invalid.onAfterEnter: ({ root }) => reset(root)`, parts by name, a repeated part as an array), and on your elements (`v-animate="{ initial: …, onAfterEnter: close }"`, or `targets.name`). `onAfterSwap` follows a swap, `onAfterMove` a `layout` slide or `animate` change, `onRepeat` sits beside `repeat`. All run at once when nothing plays, as with motion off. Never a `setTimeout` that repeats a duration; `settled()` is for tests. Table: [references/guides/motion.md](references/guides/motion.md#callbacks).

Units: `duration` is ms; `delay`, `stagger` and `spring.response` are seconds. A spring and a duration never go together. Springs by name: `'snappy'`, `'smooth'`, `'bouncy'`, `'gentle'`. Entrances by name: `fade`, `rise`, `drop`, `pop`, `blur`. Every name is a typed union, so a typo fails vue-tsc.

## Upgrading from 0.4

0.5.0 replaced the old animation hand-off. Code written for 0.4, or remembered from it, breaks on these:

- `motionCss` / `motion-css` is now `motion`.
- `useSpringValue`, `createSpring` and `stepSpring` import from `vael-ui/motion`.
- `beforeClose(done)` is gone: `before-close` returns `boolean` or a promise, and exit animations move to `motion.leave`.
- `@item-enter`, `@card-leave`, `@row-enter`, `@chip-leave`, `@action-enter` and the other list events are gone: use `enter` / `leave` hooks in `motion`.
- Button colors come from `--ui-button-bg` / `--ui-button-border`; Sortable's root is a `div.ui-sortable` around `ul.ui-sortable-list`; Tooltip's label is in `.ui-tooltip-content`; list hover backgrounds are one `.ui-indicator` styled by `--ui-indicator-bg`.

The [changelog](./references/guides/changelog.md) has every release in full, with before and after code.

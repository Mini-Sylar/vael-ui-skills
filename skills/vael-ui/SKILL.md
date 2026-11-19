---
name: vael-ui
description: Use whenever writing, editing, reviewing or debugging Vue code that uses vael-ui ("vael-ui" or "vael-ui/vapor" imports, or vael-ui in package.json), even when the request only names a component (dialog, drawer, toast, data table, select, date picker, sortable board, calendar planner, tree) or asks to animate one. Covers the exact props, slots, events and motion parts of all 70 components; setup (vdom vs Vapor builds, auto-import, Nuxt, SSR); theming with ConfigProvider, the ui prop and --ui-* variables; the motion system (the `motion` prop, springs, states, `v-animate` with layout, `layoutId`, `appear`, `after`, live values; hooks for GSAP or motion; settled() in tests) with rules and recipes for motion that feels right; gestures (`useDrag`, `useWheel`, `usePanZoom`, `useDragSelect`, `v-draggable`, sortable boards); imperative composables (openDialog, confirmAction, toast); and upgrading from 0.4. Check it before guessing a prop name, hand-building something vael-ui ships, or animating around a vael-ui component.
license: MIT
metadata:
  author: Mini-Sylar
  version: 2.3.0
---

# vael-ui `vael-ui@0.5.5`

A Vue 3 component library: 70 components, from buttons and form fields to overlays, data tables, drag-and-drop and gestures, plus composables, with one motion system you take over part by part. Two builds from one source: `vael-ui` (regular Vue) and `vael-ui/vapor` (Vue Vapor), same names, props and composables.

**References** (generated from the library's types; if this page disagrees with one, the reference is right):
[Components](./references/docs/_INDEX.md) · [Composables](./references/composables/_INDEX.md) · [Motion guide](./references/guides/motion.md) · [Motion recipes](./references/guides/recipes.md) · [Auto import](./references/guides/auto-import.md) · [Styling & CSS variables](./references/guides/styling-and-layers.md) · [Changelog](./references/guides/changelog.md)

## How to work with this skill

1. **Open the component's reference file before writing a line.** It lists every prop, slot, event, exposed value and motion part. Never guess a prop name; a typo in a motion value fails `vue-tsc`, a typo in a prop silently does nothing.
2. **Walk the build sequence below in order.** Setup → pick the component → wire it → style it → animate it → test it. Most mistakes come from skipping a step: hand-building something that ships, styling a part with a deep selector, or wrapping a vael-ui panel in `<Transition>`.
3. **Make the call, write the code.** Don't present option menus. State in one line what you chose when it wasn't obvious.
4. **Read the project's taste first.** If the repo has a `DESIGN.md` (or a CLAUDE.md / AGENTS.md section) with Motion or Interaction rules, they beat the defaults here. When the user corrects a duration, a spring, a distance or a look, follow it for the rest of the task and say it's worth recording (`/learn-taste` writes it down with its reason).

**Pairs with Emil Kowalski's skills, when they're installed.** `animate`, `choreograph`, `gestures`, `review-animations` and `emil-design-eng` carry the philosophy and the CSS and Motion recipes. This skill is the vael-ui mapping of the same rules: which prop, directive or composable plays each one, with the values already chosen. When they disagree on a library detail, this skill and the reference files win; when they disagree on feel, check the recording.

## Hard rules

1. **One build per app.** `vael-ui` with `createApp`, `vael-ui/vapor` with `createVaporApp`. Mixing needs Vue's `vaporInteropPlugin` and is only for migrating a vdom app a few components at a time.
2. **Never fight built-in motion with CSS or another library.** No `!important` transitions, no `transition` on a part vael-ui animates, no `<Transition>` or motion-v `<AnimatePresence>` around a vael-ui panel. Use `:motion` (off, a spring, values, hooks).
3. **Don't build what ships.** Check the [component index](./references/docs/_INDEX.md) first: gauges and donuts (`Progress type="circle"`), stacked bars (`Progress` segments), spinners (`Loader`), skeletons that draw real content as bones (`Skeleton :loading`), inline rename (`Editable`), color pickers, rolling numbers (`AnimatedNumber`), text swaps (`AnimatedText`), boards (`Sortable` + `useSortableGroup`), month planners (`Calendar planner`), tours, OTP fields, swipe actions, pull-to-refresh, hold-to-confirm (`Button hold`), sliding hover highlights (`v-hover-indicator`), scroll fades (`v-scroll-mask`).
4. **Style through the API, not selectors.** App-wide: `ConfigProvider` and `--ui-*` variables. One instance: the `ui` prop (part name → class or `{ class, style }`). Built-in icons and buttons through their `ui` key (`chevron`, `clear`, `icon`). A deep selector on a `.ui-*` class is the last resort and goes in unlayered CSS.
5. **Guards aren't animations.** `before-close` returns `true`, `false` or a promise (`() => confirmAction({ title: 'Discard?' }).result`). An exit animates through `motion.exit` or a `leave` hook. `force-mount` keeps content mounted; it hands you nothing else.
6. **Decide whether to animate before how.** Keyboard-driven and hundreds-a-day UI (command palettes, menus, tabs) keeps the built-in look or none. Name the purpose (feedback, where it came from, what changed, smoothing a jump) or don't animate.
7. **Directives are imported, not auto-imported.** `vTooltip`, `vAnimate` (`vAnimateVapor as vAnimate` in a `<script setup vapor>`), `vScrollMask`, `vHoverIndicator`, `vDraggable` from `vael-ui` or `vael-ui/motion`.
8. **Don't hand-roll a drag.** A free drag is `useDrag`, a picker or carousel `useWheel`, pan and zoom `usePanZoom`, band selection `useDragSelect`, reordering `Sortable` or `v-draggable`. They track 1:1, throw on a spring that keeps the speed, catch mid-flight, and follow `ConfigProvider` and reduced motion. No `pointermove` listeners, VueUse `useDraggable` with a CSS settle, or `setInterval` carousels.
9. **A number drives a look through a live value.** `v-animate="{ animate: { rotate: () => mapRange(drag.x.value, [-300, 300], [-20, 20]) } }"`: followed every frame with no render. Never write `style.transform` for something a getter can drive, and never put a CSS `transition` on it.
10. **In tests, `await settled()`** from `vael-ui/motion`, or wrap in `<ConfigProvider :motion="false">`. Never a `setTimeout` that repeats a duration.

## The build sequence

### 1. Set up

```sh
npm install vael-ui vue
```

Vue `^3.5.0`. The Vapor build needs Vue `^3.6.0` and `createVaporApp`; Vapor is pre-1.0, so **pin the exact prerelease** `vael-ui@0.5.5` is compiled against (`npm install vue@3.6.0-rc.15 --save-exact`). A floating `rc` tag causes compiler/runtime mismatches, often inside `<Transition>`. In Vapor a component's ref holds only what it exposes, not its element: when you pass your own component to `setTriggerEl`, `triggerEl` or `scrollEl`, give it `defineExpose({ el })`, or pass a ref to an element. Every vael-ui component with a root element exposes `el`.

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

| Need | Do |
| --- | --- |
| Not importing every component | `unplugin-vue-components` + `VaelUiResolver` from `vael-ui/resolver`: [Auto import](./references/guides/auto-import.md) |
| Whole app as Vapor | `vue({ features: { vapor: true } })` (`@vitejs/plugin-vue` 6.0.8+); `optionsAPI: false` is safe. vue-tsc types an SFC as Vapor only with the `vapor` attribute, so cast the root: `createVaporApp(App as unknown as VaporComponent)`. With the `vapor` export condition add `"customConditions": ["vapor"]` to tsconfig, and spread `defaultClientConditions` / `defaultServerConditions` in Vite's `resolve.conditions` (alone, `['vapor']` breaks Vite) |
| Toasts, `v-tooltip`, `openDialog()`, `openPopover()` | Mount `<Toaster />`, `<TooltipHost />`, `<DialogHost />`, `<PopoverHost />` once, **inside** your `ConfigProvider` so what they show takes its theme, motion, messages and size |
| Fonts | Components use `font: inherit`: set `font-family` on `body` |
| Plain Vite SSR | `ssr: { noExternal: ['vael-ui'] }`, inject `ctx.teleports?.body` before `</body>`. `v-animate` entrances are written as CSS on the server and never replay on hydration |
| Nuxt | `vael-ui/nuxt` in `modules` (registers components and `v-animate`); `vaelUi: { variant: 'vapor' }` for the Vapor build (Nuxt 4.6+, Vue 3.6 pinned). Mount the hosts in `app.vue`, import composables from `'vael-ui'`, register `v-tooltip` in a plugin: `nuxtApp.vueApp.directive('tooltip', vTooltip)` |
| Fields zooming on iOS | Call `preventFocusZoom()` once in `main.ts`: iOS stops zooming a focused field, pinch-zoom still works, and fields keep your text size. Or raise field text on touch screens with `--ui-touch-font-size: 16px` |
| `vael-ui/style.css` | Don't import it by default; each component loads its own CSS |

### 2. Pick the component

Read the reference file, then decide at the level of the whole task:

| Task | Reach for |
| --- | --- |
| Confirm, prompt, one-off dialog, toast | `confirmAction()`, `openDialog()`, `openPopover()`, `toast()`: promises and handles, no template wiring. `const { result } = confirmAction({ title: 'Delete?', variant: 'danger', onConfirm: () => api.delete(id) }); if (await result) refresh()` |
| A form | `Field` around each control (label placement, description, error, required). Controls merge a parent Field's `disabled`, `id` and `aria-describedby` on their own. To validate it, use the separate `vael-form` package (schema, `v-model`, errors, accessible submit): `<Field :error="errors.email"><Input v-model="email" v-bind="attrs.email" /></Field>`; docs at vael-ui.dev/forms. A Field error is read with its control, not `role="alert"`; vael-form focuses the first invalid field on submit. `required` on a control (Select, Combobox, DatePicker, OtpInput, …) blocks a plain form submit while empty; a Field's `required` is only the hint |
| A toggle, a toggle row | `<Button v-model:pressed>`; `ButtonGroup` with `v-model` and a `value` per `Button` (`multiple`, `:allow-empty="false"`). Never hand-wire `aria-pressed` |
| Grouped options | `{ label, items }` entries in `Select`/`Combobox` items (`ui.group`, `#group-label`). Collapsible groups are a tree: `TreeSelect` |
| An "Unassigned" or "None" option | an item with `value: null`: picking it sets the model to `null`, and `null` shows it as picked. No sentinel strings |
| A table column that must not widen the table | `<Column fill>`: takes what's left, ends long values in an ellipsis |
| A long or async list in a dropdown | `maxPanelHeight` (Menu, Combobox, TreeSelect) caps the panel; `panel-width` sizes it (`'trigger'`, px, any length). `Input loading` and `CommandPalette loading` show a spinner while results load |
| A menu row with more than text | `#label` keeps the icon, shortcut and chevron and replaces the text; `#item` replaces the whole row (Menu, MenuList, ContextMenu, SplitButton) |
| Reorder, a board | `Sortable` (`handle="item"` to drag from anywhere, `variant="plain"` for rows that look like your content, `virtualize` for hundreds of cards, `useSortableGroup` across lists, `@drag-start` / `@drag-end`). Grids reorder slot by slot with `useSortable` or `v-draggable` |
| A month of tasks | `<Calendar planner>` with a `Sortable` in `#day` per day, sharing one group |
| Loading content | `<Skeleton :loading>` around the real content, rendered with placeholder data: it draws each line, avatar and button as a bone. Mark a custom element `data-skeleton-block` |
| Pull to refresh on a page | `PullToRefresh` with `:scroll-el="document.documentElement"` and the same handler as the visible button; mount it only where touch is the input |
| A shortcut hint on a tooltip | `v-tooltip="{ content: 'Search', keys: ['⌘', 'K'] }"` |
| A tooltip with markup (status dot, progress bar) | a component as `content`: `v-tooltip="{ content: ProjectTip, props: { project } }"`, drawn in the one shared tooltip. Links and buttons go in a Popover |
| A tooltip along a wide target (timeline bar) | `v-tooltip="{ content, follow: true }"` or `<Tooltip follow>`: it tracks the pointer and stops at the ends |
| A stacked bar with hover text per segment, a thinner stop or a tiny one that stays visible | `Progress` segments take `tooltip`, `class` and `style`: `{ value: 2, tooltip: 'Stopped 2 min', style: { minInlineSize: '6px' } }`. A `gap` goes on `ui.track`; the segments shrink to fit it. Hover text needs a mounted `TooltipHost` |
| A timeline bar, a span to drag and stretch | two `useResizable(size, { offset: start, edge: 'start' \| 'end', bounds, step, scrollEl })` over the same refs; `onMovePointerdown` / `onMoveKeydown` on the bar. Snaps on release, Escape reverts: [useResizable](./references/composables/useResizable.md). A plain panel: `Resizable` (`step`) |
| A card, notice or panel that moves with the finger and decides on release (swipe to dismiss, a deck, a draggable dialog) | `useDrag({ el, axis, handle, onRelease })`: `onRelease({ projected })` returns where it goes, or `null` for home; `exit` on a spring keeps the throw's speed. `v-draggable` only reorders |
| A picker, a carousel, a coverflow | `useWheel(index, { el, count, size, orientation, loop })`: items place with `--ui-wheel-distance` and style with `progressOf(i)`; `step`, `goTo` and `onKeydown` for buttons and keys |
| A photo viewer, a map, a lightbox | `usePanZoom({ el, maxScale })`: pinch, double tap, ctrl and wheel, keys; `scale` is live. A lightbox is a `Dialog` with `maximized` |
| Drag across empty space to select | `useDragSelect({ el, spring })`: the children are the items; draw the band with the live `x`, `y`, `width`, `height` |
| A sidebar nav of links | `MenuList`: every row is its own Tab stop, like links. Your own list: `useMenu({ roving: false })` |

**Things that look like gaps but aren't:** Combobox and Select options hold no buttons (content is flattened for screen readers; put removal outside the list). A `Toolbar` needs `flex: 1` so its overflow menu can measure its room. Components inside a narrow or resizable flex container need `min-inline-size: 0` or they overflow instead of shrinking. Sortable's gap and direction live on its root (`class` or `ui.root`), never on the inner list.

### 3. Wire it

- **State is a `v-model`** per controllable thing: `v-model:open`, `v-model:items`, `v-model:editing`, `v-model:pressed`. Open or close from outside by writing the model; the component focuses what should be focused (an `Editable` opened with `v-model:editing` focuses its control).
- **Callbacks for what happens after** go on the thing that animates: an overlay's `motion.onAfterLeave` (add the row, toast, navigate there, never while it fades), a list's per-item `onAfterEnter`, a state's `onAfterEnter`. All run at once when motion is off.
- **`before-close`** is the only close guard; return a promise for async.
- **Focus on open** is handled: Dialog, Drawer and BottomSheet focus an `[autofocus]` element, else the first control in their content (not the × button). Put `autofocus` on the field you want first rather than an `initialFocus` callback; `modal: false` still takes and returns focus. `confirmAction` is an alert dialog either way; its popover opens on Cancel.
- **Toasts:** toasts with an `action` stay until dismissed (so Undo is reachable). Pass `duration` only to force a timeout; `duration: Infinity` keeps an important error up.
- **Escape** goes to the innermost thing that wants it: a tooltip, an `Editable` mid-edit, a Sortable or resize mid-drag, then the dialog. Don't add window listeners for it.
- **Clicks inside rows** of Tree, Calendar days and Sortable items reach the button you put there; the row itself doesn't also select. Don't `.stop` on every inner button.
- **vue-tsc doesn't narrow `v-if` inside a directive's value:** `` v-if="selected" v-animate="{ layoutId: `site-${selected!.id}` }" `` needs the `!`.

### 4. Style it

Theme once, override per instance, in this order:

```vue
<ConfigProvider :theme="{ primary: '#6d28d9', radius: '8px' }" size="sm" :motion="{ spring: 'snappy' }">
  <App />
</ConfigProvider>

<Dialog :ui="{ panel: 'my-panel', body: { class: 'p-0' } }" />
```

| Want | Use |
| --- | --- |
| A global change | One `--ui-*` variable on `:root` ([the table](./references/guides/styling-and-layers.md#css-variables)): `--ui-primary`, `--ui-radius`, `--ui-overlay` (every backdrop), `--ui-indicator-bg` (every list's hover highlight), `--ui-touch-font-size` |
| A Button's colours | `--ui-button-bg` / `--ui-button-border` on it. Its box is a `::before` layer, so `background` and `border` on the element vanish. Dashed: `.my-button::before { border-style: dashed }` |
| A list's hover | `--ui-indicator-bg`. Select, Menu, Tree, DataTable, Tabs slide one `.ui-indicator` behind their rows; a row `:hover` background paints over it |
| Your own list's hover | `<ul v-hover-indicator="'.row'">` (inner quotes: the value is an expression). `.over` for rows with a solid background. The highlight is the list's first child, so `:first-child` rules on rows need `:first-of-type`. On a `--ui-muted` panel set `--ui-indicator-bg: var(--ui-muted-hover)` |
| A tooltip's colours | `--ui-tooltip-bg` / `--ui-tooltip-color` in `ui.panel`'s `style` on `TooltipHost` (all) or in `v-tooltip="{ content, ui: { panel } }"` (one), or in your CSS. Its box is a `::before`, so a plain `background` there paints a square rectangle that doesn't move with the tooltip |
| Tailwind | Pass `twMerge` as `ConfigProvider`'s `class-merge`; `ui` classes don't dedupe on their own |
| A `ui` class from `<style scoped>` | `:deep(.my-class)`: the scope attribute never reaches a component's inner parts or a teleported panel |

**Cascade layers.** vael-ui's CSS is in `@layer ui-components`. Your *unlayered* CSS wins over it, whatever the specificity. A *reset* belongs in a layer declared **before** `ui-components`; an *override* must be unlayered or in a layer declared **after** it, or it loses (`.x-body { padding: 0 }` in an early layer doesn't beat `.ui-dialog-header + .ui-dialog-body`). [Styling](./references/guides/styling-and-layers.md).

### 5. Animate it

Guides: [Motion](./references/guides/motion.md) for the API by task and the rules, [Recipes](./references/guides/recipes.md) for ready-made starting points (a list, a page change, a shared element, a success sequence, swipe to dismiss, a card deck, a picker, pan and zoom). Start from the recipe that matches the request. Decide in this order, and stop at "don't":

1. **How often is it seen?** Hundreds a day or from the keyboard (command palettes, menus, tabs, list navigation): nothing beyond the built-in. Occasionally (dialogs, drawers, toasts): standard. Rarely (onboarding, success, celebration): delight is allowed. Say which tier in your answer.
2. **What's it for?** Feedback, where it came from, what changed, smoothing a jump. Name it in a word, or don't build it. Data people are reading or acting on doesn't move for style.
3. **Cast the elements** when more than one moves: one lead (moves first and furthest), support (follows with less), decoration (fades only), everything else still. Then relate each pair: together by default; overlap (a stagger, 30 to 80 ms, total under about 300 ms) only when order carries meaning; sequence (`after`, `animate([…])`) only when overlap would look wrong. One entrance per container: a panel that animates in carries its content. Exits go together, shorter than the entrance.
4. **Which tool?** Walk down and stop at the first that fits:

| The motion is | Tool |
| --- | --- |
| A vael-ui component's own | its `motion` prop: off, a spring, values by part, states, hooks |
| Your element coming, going, moving or holding a look | `v-animate`: `initial` / `exit`, `layout`, `layoutId`, `states`, `animate` |
| Started by an event (a wrong code, a sent payment) | `animate()` / `useAnimate()`, or a list of steps for a sequence |
| Following a number (a drag, a scroll, the pointer, a slider) | a live value in `animate`, with `mapRange`; `progress` for a keyframe sweep |
| A number you display or draw | `AnimatedNumber`, or `useTween` |
| Driven by a finger | `useDrag`, `useWheel`, `usePanZoom`, `useDragSelect` |
| The user's own library (GSAP, motion) | `enter` / `leave` hooks that **return** the animation: vael-ui waits for it and stops it if interrupted |

```vue
<Dialog v-model:open="open" :motion="false" />                                           <!-- off -->
<Drawer v-model:open="open" :motion="{ spring: 'gentle' }" />                             <!-- same look, other feel -->
<Dialog :motion="{ initial: { panel: { opacity: 0, y: 16, scale: '0.96' } } }" />        <!-- values by part (the ui keys) -->
<Input :invalid="invalid" :motion="{ states: { invalid: { root: { x: [0, -6, 6, -3, 0] } } } }" />
<Popover open-on-trigger-click :motion="{ morph: 'trigger' }">…</Popover>                <!-- grows out of its button -->

<li v-for="task in tasks" :key="task.id" v-animate:tasks.rise.stagger="{ layout: true }"> <!-- modifiers pick, the value tunes -->
<span v-animate.pop.bouncy>New</span>
<div v-animate="{ states: { hover: { scale: 1.03 }, active: { scale: 0.97 } } }">…</div>
<li v-for="p in projects" :key="p.id" v-animate:projects="{ initial: { opacity: 0, y: 6 }, stagger: true, appear: 'once' }"> <!-- once per page load -->
<article v-animate="{ animate: { rotate: () => mapRange(drag.x.value, [-300, 300], [-20, 20]) } }"> <!-- a live value -->

<RouterView v-slot="{ Component, route }">                                                <!-- pages: out, then in -->
  <component :is="Component" :key="route.path" v-animate:page="{ initial: { opacity: 0, y: 8 }, swap: 'wait' }" />
</RouterView>
```

```ts
// Your own library: return the animation; vael-ui waits for it and stops it if interrupted
const motion: DialogMotion = {
  enter: ({ panel }) => gsap.from(panel, { opacity: 0, y: 16, duration: 0.3, ease: 'power3.out' }),
  leave: ({ panel }) => gsap.to(panel, { opacity: 0, duration: 0.12 }),
}
```

**Imports.** Components and their motion types (`DialogMotion`, `SelectPart`) from `vael-ui`, along with the gesture composables (`useDrag`, `useWheel`, `usePanZoom`, `useDragSelect`). Everything for your own elements from `vael-ui/motion`, in both builds: `vAnimate`, `animate`, `useAnimate`, `useMotion`, `mapRange`, `useScrollProgress`, `useScrollLength`, `useTween`, `settled`, `springEasing`, `useSpringValue`, and the types `MotionValues`, `MotionTiming`, `MotionStyle`. Inside a component use `useAnimate()`; plain `animate` is for helpers and tests that have an element.

**Names.** `v-animate:rows` names an element. Elements under one name that come in together stagger as one group; inside a component they're its `targets.rows`; `after: 'rows'` (or `'rows:60%'`) waits for them, for what they're already playing, or for a `layoutId` slide under that name; pages under one name can `swap: 'wait'`; `appear: 'once'` plays the name's entrance once per page load. Name an element only when something else has to point at it.

**Values, by what moves** (the full table is [here](./references/guides/motion.md#5-durations-easing-and-springs)):

| What | Enter | Leave |
| --- | --- | --- |
| Tooltip | ~125 ms, `scale: '0.98'` | faster |
| Menu, select | 150–250 ms, `scale: '0.97'`, 6–8 px | faster |
| Dialog | 200–250 ms, `scale: '0.96'`, 12–16 px | faster |
| Drawer, sheet, anything grabbable | a spring: `'snappy'`, `'smooth'` (no overshoot); `'bouncy'` only after a flick | never bounces |

Units: `duration` ms; `delay`, `stagger`, `spring.response` seconds. A spring and a duration never go together. Tokens by name: `easing: 'out'`, `duration: 'enter'`. Entrances by name: `fade`, `rise`, `drop`, `pop`, `blur`. Every name is a typed union.

Rules that apply every time: ease out both ways and leave faster than you enter, under 300 ms; never scale from 0 (`'0.95'` or higher with `opacity: 0`, move 4–16 px); delay only to stagger, to filter an accidental trigger, or to sequence with `after` (a stagger's total under about 300 ms; a close is never delayed); animate `x`, `y`, `scale`, `rotate`, `opacity`, never `width`/`height`/`top` (use `.size`, `resize` or `layout`); when a panel opens above a list, give the list `layout: 'position'` too or it snaps; turn a trigger only with `v-animate` (another library writing `transform` on it drags its open panel along; tween a child instead); reduced motion is handled for you (fades kept, movement dropped, hooks skipped unless `reducedMotion: 'run'`).

**Gestures follow one split.** While the pointer is down the element is the finger: 1:1, no easing, no spring, no `transition`, and no render per move (read `x`, `y`, `scale`, `position` in getters). On release a spring keeps the speed: decide from `projected` (where the throw would land), so a short flick counts; bounce only after a throw, never for a button or a key; a new press catches it mid-flight. Everything linked to the drag (the page behind, the next card, the backdrop) reads the one number the drag writes. A drag has no keyboard form: add `throwTo`, `step` and `onKeydown`. Give a thrown element's `exit` a spring (`x: { value: far, spring: 'smooth' }`) or it stops at the edge and only then fades.

### 6. Test it

`await settled()` after the interaction, or `<ConfigProvider :motion="false">` around the fixture. Named `v-animate` elements keep per-name state for the page load (`appear: 'once'`), so give a fixture's names a suffix the test owns.

## Never ship

| Never | Instead |
| --- | --- |
| `<Transition>` / `<AnimatePresence>` around a vael-ui panel | `:motion` |
| `beforeClose(done)`, `@item-enter`, `motion-css` | 0.5 API: `before-close` returns a boolean or promise; `motion.enter` / `leave` |
| A card inside `#handle` to drag from anywhere | `handle="item"` |
| `:hover` backgrounds on rows of a list | `--ui-indicator-bg`, or `v-hover-indicator` on your own list |
| `background` / `border` on `.ui-button` | `--ui-button-bg` / `--ui-button-border` |
| `background` on a Dialog's `ui.overlay` | `--ui-overlay` |
| An override class inside an app layer declared before `ui-components` | unlayered, or a later layer |
| `setTimeout(duration)` to act after motion | `onAfterEnter` / `onAfterLeave` / `onAfterMove`, `settled()` in tests |
| `window.addEventListener('keydown')` for Escape | the component already handles it (Editable, Sortable, overlays) |
| `import 'vael-ui/style.css'` | nothing: components load their own |
| `pointermove` / VueUse `useDraggable` plus a CSS transition to settle, `setInterval` carousels | `useDrag`, `useWheel`, `usePanZoom`, `useDragSelect` |
| A spring or `transition` on an element while it's dragged | nothing: tracking is 1:1, the composable springs it on release |
| Deciding a swipe from the offset alone, or an `exit` with a plain number after a throw | `projected` in `onRelease`; `x: { value, spring: 'smooth' }` |
| `v-draggable` to move one element, or `useDrag` to reorder | `v-draggable` / `Sortable` reorder; `useDrag` moves one element |
| `el.style.transform` written for something a getter can drive | a live value: `animate: { rotate: () => … }` |
| Stagger on a menu, a command palette, tabs or any keyboard path | the built-in look, or none |
| A prop name from memory | the reference file |
| `setTriggerEl`, `:trigger-el` or `:scroll-el` given your own Vapor component | an element ref, or `defineExpose({ el })` in that component |

## Upgrading from 0.4

0.5 replaced the animation hand-off. Code written for 0.4 breaks on these: `motionCss` is `motion`; `useSpringValue`, `createSpring`, `stepSpring` import from `vael-ui/motion`; `beforeClose(done)` is `before-close` returning `boolean | Promise` with exits in `motion.leave`; the list events (`@item-enter`, `@card-leave`, `@row-enter`, …) are `enter` / `leave` hooks in `motion`; Button colours come from `--ui-button-bg` / `--ui-button-border`; Sortable's root is `div.ui-sortable` around `ul.ui-sortable-list`; Tooltip's label is in `.ui-tooltip-content`; list hover backgrounds are one `.ui-indicator`. The [changelog](./references/guides/changelog.md) has every release with before and after code.

## Output

Write the code. Then, in a few lines:

- **The gate**: the frequency tier and the named purpose. If part of the request was declined (a stagger on a menu, motion on a keyboard path), say which and why.
- **The ingredients**: the component or composable and why (one line), then tool, values and timing in one line each.
- **What you decided** that the request left open.
- **What to feel-check**, when the result depends on motion: play it at 2 to 5 times the duration, step it frame by frame, interrupt it mid-flight, try a gesture on a real phone, and look again the next day. If the user corrects any value, that's taste worth recording with `/learn-taste`.

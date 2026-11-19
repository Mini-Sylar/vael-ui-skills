---
name: vael-ui
description: Use whenever writing, editing, reviewing or debugging Vue code that uses vael-ui ("vael-ui" or "vael-ui/vapor" imports, or vael-ui in package.json), even when the request only names a component (dialog, drawer, toast, data table, select, date picker, sortable board, calendar planner, tree). Covers the exact props, slots, events and motion parts of all 70 components; setup (vdom vs Vapor builds, auto-import, Nuxt, SSR); theming with ConfigProvider, the ui prop and --ui-* variables; imperative composables (openDialog, confirmAction, toast); and upgrading from 0.4. Motion beyond a component's own `motion` prop (v-animate, gestures, springs, GSAP) is the `vael-motion` skill. Check this one before guessing a prop name or hand-building something vael-ui ships.
license: MIT
metadata:
  author: Mini-Sylar
  version: 2.4.0
---

# vael-ui `vael-ui@0.5.5`

A Vue 3 component library: 70 components, from buttons and form fields to overlays, data tables, drag-and-drop and gestures, plus composables, with one motion system you take over part by part. Two builds from one source: `vael-ui` (regular Vue) and `vael-ui/vapor` (Vue Vapor), same names, props and composables.

**References** (generated from the library's types; if this page disagrees with one, the reference is right):
[Components](./references/docs/_INDEX.md) · [Composables](./references/composables/_INDEX.md) · [Motion guide](./references/guides/motion.md) · [Motion recipes](./references/guides/recipes.md) (both read through `vael-motion`) · [Setup](./references/guides/setup.md) · [Auto import](./references/guides/auto-import.md) · [Styling & CSS variables](./references/guides/styling-and-layers.md) · [Other exports](./references/guides/exports.md) (helpers, pure functions, injection keys, every type) · [Changelog](./references/guides/changelog.md)

## How to work with this skill

1. **Open the component's reference file before writing a line.** It lists every prop, slot, event, exposed value and motion part. Never guess a prop name; a typo in a motion value fails `vue-tsc`, a typo in a prop silently does nothing.
2. **Walk the build sequence below in order.** Setup → pick the component → wire it → style it → animate it → test it. Most mistakes come from skipping a step: hand-building something that ships, styling a part with a deep selector, or wrapping a vael-ui panel in `<Transition>`.
3. **Make the call, write the code.** Don't present option menus. State in one line what you chose when it wasn't obvious.
4. **Read the project's taste first.** If the repo has a `DESIGN.md` (or a CLAUDE.md / AGENTS.md section) for surfaces, colour, spacing or interaction, it beats the defaults here. When the user corrects a value, follow it and say it's worth recording with `/learn-taste`.
5. **Anything that moves beyond a component's own `motion` prop** (your elements, a stagger, a drag, a carousel, GSAP) is the `vael-motion` skill: read it, and its gate, before writing motion.

## Hard rules

1. **One build per app.** `vael-ui` with `createApp`, `vael-ui/vapor` with `createVaporApp`. Mixing needs Vue's `vaporInteropPlugin` and is only for migrating a vdom app a few components at a time.
2. **Never fight built-in motion with CSS or another library.** No `!important` transitions, no `transition` on a part vael-ui animates, no `<Transition>` or motion-v `<AnimatePresence>` around a vael-ui panel. Use `:motion` (off, a spring, values, hooks).
3. **Don't build what ships.** Check the [component index](./references/docs/_INDEX.md) first: gauges and donuts (`Progress type="circle"`), stacked bars (`Progress` segments), spinners (`Loader`), skeletons that draw real content as bones (`Skeleton :loading`), inline rename (`Editable`), color pickers, rolling numbers (`AnimatedNumber`), text swaps (`AnimatedText`), boards (`Sortable` + `useSortableGroup`), month planners (`Calendar planner`), tours, OTP fields, swipe actions, pull-to-refresh, hold-to-confirm (`Button hold`), sliding hover highlights (`v-hover-indicator`), scroll fades (`v-scroll-mask`).
4. **Style through the API, not selectors.** App-wide: `ConfigProvider` and `--ui-*` variables. One instance: the `ui` prop (part name → class or `{ class, style }`). Built-in icons and buttons through their `ui` key (`chevron`, `clear`, `icon`). A deep selector on a `.ui-*` class is the last resort and goes in unlayered CSS.
5. **Guards aren't animations.** `before-close` returns `true`, `false` or a promise (`() => confirmAction({ title: 'Discard?' }).result`). An exit animates through `motion.exit` or a `leave` hook. `force-mount` keeps content mounted; it hands you nothing else.
6. **Decide whether to animate before how.** Keyboard-driven and hundreds-a-day UI (command palettes, menus, tabs) keeps the built-in look or none. Name the purpose (feedback, where it came from, what changed, smoothing a jump) or don't animate.
7. **Directives are imported, not auto-imported.** `vTooltip`, `vAnimate` (`vAnimateVapor as vAnimate` in a `<script setup vapor>`), `vScrollMask`, `vHoverIndicator`, `vDraggable` from `vael-ui` or `vael-ui/motion`.
8. **In tests, `await settled()`** from `vael-ui/motion`, or wrap in `<ConfigProvider :motion="false">`. Never a `setTimeout` that repeats a duration.

## The build sequence

### 1. Set up

```sh
npm install vael-ui vue
```

Vue `^3.5.0`. One build per app: `vael-ui` with `createApp`, or `vael-ui/vapor` (Vue `^3.6.0`, `createVaporApp`, the exact prerelease `vael-ui@0.5.5` is compiled against, `vue@3.6.0-rc.15`). Mount `<Toaster />`, `<TooltipHost />`, `<DialogHost />` and `<PopoverHost />` once, **inside** your `ConfigProvider`, when you use `toast()`, `v-tooltip`, `openDialog()` or `openPopover()`. Don't import `vael-ui/style.css`: each component loads its own CSS. Everything else (auto-import, a whole-app Vapor build and its tsconfig, Nuxt, plain Vite SSR, fonts, fields zooming on iOS) is in [Setup](./references/guides/setup.md); read it when you start an app or change how it builds.

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
| A timeline bar, a span to drag and stretch | one `useResizable(size, { offset: start, bounds, step, scrollEl })`; each end's handle names its edge, `bar.onHandlePointerdown($event, 'start' \| 'end')`, and the bar takes `onMovePointerdown` / `onMoveKeydown`. With the component: `<Resizable edge="both" v-model:size="size" v-model:offset="start" :bounds :step :scroll-el @commit>`, a bar that positions itself, moves when dragged and has a handle on each end. A panel pulled from either side without `offset`: `edge="both"` pins the far edge and stops at its container; `centered` resizes from the middle instead (the pulled edge follows the pointer, the other mirrors it). `flip: false` on `useResizable` stops a handle at `min` instead of flipping the span. Snaps on release, Escape reverts: [useResizable](./references/composables/useResizable.md). A plain panel: `Resizable` (`step`) |
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

Each component animates itself. Keep it, retime it, or take a part over with `:motion`; never wrap a vael-ui panel in `<Transition>` or fight a part's motion with CSS. For anything else (your own elements, a list or page, a gesture, GSAP or motion), use the **`vael-motion`** skill: it runs the gate (how often it's seen, what it's for), casts the elements, picks the tool and the values.

```vue
<Dialog v-model:open="open" :motion="false" />                                    <!-- off -->
<Drawer v-model:open="open" :motion="{ spring: 'gentle' }" />                      <!-- same look, other feel -->
<Dialog :motion="{ initial: { panel: { opacity: 0, y: 16, scale: '0.96' } } }" />  <!-- values by part (the ui keys) -->
<Input :invalid="invalid" :motion="{ states: { invalid: { root: { x: [0, -6, 6, -3, 0] } } } }" />
<Popover open-on-trigger-click :motion="{ morph: 'trigger' }">…</Popover>          <!-- grows out of its button -->
```

Keyboard-driven and hundreds-a-day UI (command palettes, menus, tabs) keeps the built-in look or none, and gets no stagger. Units: `duration` ms; `delay`, `stagger`, `spring.response` seconds. Part names are the component's `ui` keys; the reference file's Motion section lists them.

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
| A prop name from memory | the reference file |
| `setTriggerEl`, `:trigger-el` or `:scroll-el` given your own Vapor component | an element ref, or `defineExpose({ el })` in that component |

## Upgrading from 0.4

0.5 replaced the animation hand-off. Code written for 0.4 breaks on these: `motionCss` is `motion`; `useSpringValue`, `createSpring`, `stepSpring` import from `vael-ui/motion`; `beforeClose(done)` is `before-close` returning `boolean | Promise` with exits in `motion.leave`; the list events (`@item-enter`, `@card-leave`, `@row-enter`, …) are `enter` / `leave` hooks in `motion`; Button colours come from `--ui-button-bg` / `--ui-button-border`; Sortable's root is `div.ui-sortable` around `ul.ui-sortable-list`; Tooltip's label is in `.ui-tooltip-content`; list hover backgrounds are one `.ui-indicator`. The [changelog](./references/guides/changelog.md) has every release with before and after code.

## Output

Write the code. Then, in a few lines: which component or composable you used and why (one line), anything you decided that the request left open, and what to check in the browser when the result depends on look or motion.

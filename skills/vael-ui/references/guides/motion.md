# Motion

Every vael-ui component moves on one motion system. The built-in look works as it is. One `motion` prop changes it, and you can take over any part of it: the timing, a part's values, a state, the whole enter or exit, or the engine that plays it.

Read this before you write any animation around a vael-ui component. The first half is the API by task. The second half is how to make motion feel right.

Each component's reference file ends with a **Motion** section: its motion type, its parts, its states and the keys its `motion` takes. Part names are the component's `ui` keys.

## Contents

- [Units](#units)
- [The API by task](#the-api-by-task): [turn it off](#turn-it-off), [retime with a spring](#retime-with-a-spring), [change a part's values](#change-a-parts-values), [states](#states), [entrances on any component](#entrances-on-any-component), [lists](#lists), [overlays](#overlays), [your elements inside an overlay](#your-elements-inside-an-overlay), [`v-animate`](#v-animate), [from code](#from-code), [tween a number](#tween-a-number), [pages](#pages), [scroll](#scroll), [hooks for GSAP or motion](#hooks-for-gsap-or-motion), [callbacks](#callbacks), [indicators](#indicators), [your own engine](#your-own-engine), [app-wide styles](#app-wide-styles), [`useMotion`](#usemotion-for-your-own-components), [testing](#testing), [server rendering](#server-rendering), [what never animates](#what-never-animates), [debugging](#debugging)
- [Motion that feels right](#motion-that-feels-right)
- [Common mistakes](#common-mistakes)

## Units

| Value | Unit |
| --- | --- |
| `duration` | milliseconds, or a token by name such as `'enter'` |
| `delay`, `stagger`, `spring.response` | seconds |
| `x`, `y`, `blur` as a number | px (`x` mirrors in right-to-left layouts) |
| `rotate` as a number | degrees |
| `opacity`, `scale` | no unit; two-axis scale is a string: `scale: '0.9 1.1'` |
| any other CSS property | as CSS writes it (`borderRadius: '12px'`, `backgroundColor: '#22c55e'`) |

A string goes straight to CSS, so `y: '100%'` and `x: '2rem'` work.

**Timing** is a `spring` **or** a `duration` with an `easing`, never both (TypeScript rejects it). A spring takes a name, `'snappy'` (quick), `'smooth'` (calm), `'bouncy'` (overshoots) or `'gentle'` (slow, a little give), or its numbers `{ damping, response }`: `damping: 1` lands without overshoot, below 1 it bounces. An easing is any CSS easing, or a token by name: `'out'`, `'in-out'`, `'drawer'`, `'pop'`, `'bounce'` (lands like a dropped ball, for one-off moments), `'text'`. `response` is roughly the seconds to arrive. Movement and sizes ride the spring; opacity, color and blur always ease, so a bouncy spring never flickers transparency. Timing on one component retimes only its own transitions: a token named inside it still means the theme's.

Any single value can carry its own timing: `y: { value: 12, spring: { damping: 0.6, response: 0.4 }, delay: 0.05 }`.

An array is **keyframes**: `x: [0, -8, 8, -4, 0]`. With a timed value, `times` places them (0 to 1), `easing` takes one curve or one per step (a step can be a spring: `easing: ['linear', 'bouncy']`), `repeat` plays them again (`Infinity` loops while the state is on), and `repeatType: 'reverse'` alternates. Change a loop's `duration` while it plays (a value computed from your data) and it speeds up or slows down from where it is, as GSAP's `timeScale` does.

## The API by task

### Turn it off

```vue
<!-- One component: opens, closes and changes at once -->
<Dialog v-model:open="open" :motion="false" />

<!-- Everything inside: tests, screenshots, a "reduce motion" setting -->
<ConfigProvider :motion="false">
  <App />
</ConfigProvider>
```

On `ConfigProvider`, `false` also zeroes the CSS duration tokens and makes sliding indicators jump. Exposed elements (`panelEl`, `thumbEl`, `fillEl`, `highlightEl`, `parts`) stay available for your own animation.

People who ask their system for reduced motion already get fades without movement. You don't need `false` for that.

### Retime with a spring

Keep the built-in look and change only how it moves.

```vue
<!-- One component -->
<Drawer v-model:open="open" :motion="{ spring: 'gentle' }" />

<!-- The whole app: drawers, sheets, toasts, AnimatedNumber, sliding indicators, v-animate -->
<ConfigProvider :motion="{ spring: 'snappy' }">
  <App />
</ConfigProvider>
```

Bind the values to refs to retune live. Crossfades such as AnimatedText keep their own short timing. `ConfigProvider`'s `indicator` sets the sliding indicators on their own: `'glide'` (default), `'instant'`, or a spring.

On a component, a `duration` and `easing` work the same way: `:motion="{ duration: 180, easing: 'out' }"`. `ConfigProvider` takes only a spring, or a full style (see [App-wide styles](#app-wide-styles)).

For your own CSS or `element.animate()`, `springEasing(spring)` turns a spring into a `linear()` easing and its settle time in ms:

```ts
import { springEasing } from 'vael-ui/motion'

const { easing, duration } = springEasing({ damping: 0.8, response: 0.35 })
el.animate([{ transform: 'translateY(8px)' }, { transform: 'none' }], { duration, easing })
```

### Change a part's values

`initial` is where a part comes in from; `exit` is where it leaves to. Both are keyed by part name. Import the component's motion type so parts autocomplete.

```ts
import type { DialogMotion } from 'vael-ui'

const motion: DialogMotion = {
  initial: { panel: { opacity: 0, y: 24, scale: '0.96' } },
  exit: { panel: { opacity: 0, scale: '0.98' } },
  spring: { damping: 1, response: 0.35 },
}
```

```vue
<Dialog v-model:open="open" title="Invite a teammate" :motion="motion" />
```

Leave `exit` unset and the part leaves quietly: a fade with a slight shrink (`scale: '0.97'`). Closes are capped at about 200 ms whatever the spring.

A value written as a function receives `where`: `direction` in a swap (`1` forward, `-1` back), `index` and `count` in a list.

### States

`states` animates a part while the component is in a state. A plain value is **held** while the state is on. An array **plays once** each time the state turns on.

```ts
import type { AccordionMotion, ButtonMotion, InputMotion, SwitchMotion } from 'vael-ui'

// Pressed: held while pressed
const press: ButtonMotion = { states: { active: { root: { scale: '0.96' } } } }

// Invalid: plays once on entering the state
const shake: InputMotion = { states: { invalid: { root: { x: [0, -6, 6, -3, 0] } } } }

// And again on each wrong code while it stays invalid: `replay` plays it again while it's on
const again: InputMotion = {
  states: { invalid: { root: { x: [0, -6, 6, -3, 0] }, replay: attempts.value } },
}

// Open: the chevron turns, on a spring
const chevron: AccordionMotion = {
  states: { open: { chevron: { rotate: 180 } } },
  spring: { damping: 1, response: 0.3 },
}

// Pressed: the thumb squashes
const squash: SwitchMotion = { states: { active: { thumb: { scale: '1.15 0.9' } } } }
```

To shake again on a second failed submit, the state has to turn off and on: set `invalid` to `false`, then back to `true` on the next frame.

Each component's reference lists its states. Some to know: `hover` reacts to a mouse only, never to touch. `active` is pressed (pointer, Space or Enter). Switch, Checkbox and Radio have `unchecked`, so turning one off can animate too. Toaster plays its states (`loading`, `success`, `error`, `warning`, `info`) when a toast changes variant, such as a promise toast that ends in `error`.

**Repeated parts** play their states one element at a time: a Stepper's `current`, `completed` and `upcoming` circles, each Dock item's `hover` and `selected`, each OtpInput `cell`. Write the value as a function to use the element's `index`:

```ts
import type { OtpInputMotion, StepperMotion } from 'vael-ui'

// Only the step that becomes current pops
const steps: StepperMotion = {
  states: {
    current: { circle: { scale: { value: [1, 1.15, 1], duration: 400, easing: 'ease-out' } } },
  },
}

// A wrong code ripples cell by cell
const code: OtpInputMotion = {
  states: {
    invalid: { cell: ({ index }) => ({ x: { value: [0, -4, 4, 0], duration: 240, delay: index * 0.03 } }) },
  },
}
```

**Collapsible and Accordion.** Their content stays mounted; closed, it's `hidden="until-found"`, so the browser's find reaches it and opens it. Collapsible takes `enter` and `leave` hooks (`({ panel, body }) => …`): the panel shows at once and your hook animates it; closing waits for what `leave` returns.

### Entrances on any component

Every component can animate in when it mounts. It's opt-in, and it never plays on hydration.

```vue
<Card :motion="{ initial: { root: { opacity: 0, y: 8 } } }">…</Card>

<!-- Repeated parts come in one after another -->
<Rating v-model="stars" :motion="{ initial: { item: { opacity: 0, scale: '0.9' } }, stagger: 0.04 }" />
```

`replay` plays the entrance again whenever its value changes, without remounting. `appear: false` plays it only on replays:

```ts
import { computed, shallowRef, watch } from 'vue'
import type { RatingMotion } from 'vael-ui'

const stars = shallowRef(3)
const bursts = shallowRef(0)
watch(stars, (value) => value === 5 && bursts.value++)

const motion = computed<RatingMotion>(() => ({
  initial: { item: { scale: [1, 1.4, 0.95, 1] } },
  duration: 450,
  stagger: 0.05,
  appear: false,
  replay: bursts.value,
}))
```

Value components (AnimatedNumber, Progress, Slider) take `when: 'visible'`: they wait until they scroll into view.

```vue
<AnimatedNumber :value="revenue" :motion="{ when: 'visible' }" />
<Progress :value="72" :motion="{ when: 'visible' }" />
```

### Lists

Toaster (`toast`), Timeline (`item` and its `marker`, `connector`, `previousConnector`, `content`, `opposite`), FileUpload (`item`), SpeedDial (`action`), DataTable (`row`), AvatarGroup (`avatar`), Breadcrumb (`item`), CommandPalette (`motion.items`, part `item`) and the Select family's chips (`motion.chips`, part `chip`) animate items that come and go.

| Key | What it does |
| --- | --- |
| `initial` / `exit` | where items come from and go to |
| `stagger` | items arriving together one after another: seconds, `true` (the 40ms token), or `'end'`, `'center'`, `'edges'` |
| `leaving: 'pop'` (default) | lifts a leaving item out of the layout, so the rest close the gap at once |
| `leaving: 'hold'` | keeps the gap until the item is gone |
| `appear: true` | also animates the items already there when the list mounts (never on hydration) |
| `enter` / `leave` | hooks that get the item by its part name |

The other items always slide to their new places.

```ts
import type { TimelineMotion, ToasterMotion } from 'vael-ui'

const steps: TimelineMotion = {
  initial: { item: { opacity: 0, y: 8 } },
  stagger: 0.05,
  leaving: 'hold',
}

// Items leaving together go last to first
const toasts: ToasterMotion = {
  initial: { toast: { opacity: 0, y: -12 } },
  exit: { toast: ({ index, count = 1 }) => ({ opacity: { value: 0, delay: (count - 1 - index) * 0.03 } }) },
}
```

```vue
<Select v-model="tags" :items="all" multiple :motion="{ chips: { initial: { chip: { opacity: 0, scale: '0.9' } } } }" />
```

DataTable over 150 rows changes instantly either way. Rows recycled by a virtual list never animate.

### Overlays

Dialog, Drawer, BottomSheet, CommandPalette, Popover, Menu, ContextMenu, SplitButton, Tooltip, Tour, ColorPicker, Select, Combobox, CascadeSelect, DatePicker and TreeSelect open and close on the motion system.

**Grow from the trigger.** `morph: 'trigger'` grows the panel out of the element that opened it and shrinks it back on close. The trigger hides while open, as if it became the panel.

```vue
<Popover v-model:open="open" open-on-trigger-click :motion="{ morph: 'trigger' }">
  <template #trigger>
    <Button variant="outline">Feedback</Button>
  </template>
  …
</Popover>
```

**Grow from any element**, such as the row that was clicked. `hide: false` keeps it on screen:

```ts
let row: HTMLElement | null = null
const motion: DialogMotion = { morph: { from: () => row, hide: false } }

function show(event: MouseEvent) {
  row = event.currentTarget as HTMLElement
  open.value = true
}
```

Dialog also has `origin="trigger"`: it zooms the centered panel out of the focused element that opened it.

**Inner parts.** On Dialog, Drawer and BottomSheet, `initial` reaches `title`, `description`, `body`, `footer` and the rest. `stagger` brings the parts you give an `initial` in one after another, in page order (`stagger: true`). `exitOrder: 'reverse'` closes them before the panel.

**Drawer sides.** Every side is an edge: a `top` or `bottom` Drawer spans that edge and slides in from it, as left and right do. A Dialog at `position="top"` or `"bottom"` stays a card.

```ts
const motion: DialogMotion = {
  initial: {
    title: { opacity: { value: 0, delay: 0.06 }, y: { value: 6, delay: 0.06 } },
    body: { opacity: { value: 0, delay: 0.1 }, y: { value: 6, delay: 0.1 } },
    footer: { opacity: { value: 0, delay: 0.14 }, y: { value: 6, delay: 0.14 } },
  },
  exitOrder: 'reverse',
}
```

**Move the page with it.** `linked` (Dialog, Drawer, BottomSheet) moves elements outside the overlay while it's open, on the overlay's spring, and follows a sheet's drag:

```ts
const stage = useTemplateRef<HTMLElement>('stage')
const motion: DialogMotion = {
  linked: { page: { el: () => stage.value, open: { scale: '0.95', borderRadius: '12px' } } },
}
```

**Change what's inside.** Put one keyed root in the slot and change its key. `swap: 'wait'` lets the old one leave first (`'sync'`, the default, crossfades). `direction` reaches values written as functions:

```vue
<Dialog v-model:open="open" :title="steps[step].title" :motion="{ swap: 'wait', direction }">
  <div :key="step" v-animate="{ initial: ({ direction }) => ({ opacity: 0, x: 24 * direction }) }">
    {{ steps[step].text }}
  </div>
</Dialog>
```

**Size changes.** Dialog and Popover ease to new content sizes. `resize: false` snaps instead.

**Popover `surface`** draws a shapes-only layer behind the trigger and the panel, so a filter such as a goo never touches text. Define filters as data in a style: `filters: { goo: { blur: 8, contrast: [18, -7] } }`, then `surface: { filter: 'url(#goo)' }`.

**Ask before closing.** `before-close` is a guard. Return `true` to close, `false` (or nothing) to stay open, or a promise of either. While it decides, the overlay holds still. It gets `{ reason, value, from, to, progress, velocity }`.

```ts
async function guard() {
  if (!note.value) return true
  return (await confirmAction({ title: 'Discard changes?', variant: 'danger' }).result) === true
}
```

```vue
<Dialog v-model:open="open" title="New note" :before-close="guard">…</Dialog>
```

`before-close` never runs an animation. To animate a close, use `exit` or a `leave` hook.

**Keep content mounted.** `force-mount` keeps the content and its state while closed (shown with `v-show`). The built-in look then doesn't play; give `motion` values or hooks if you want it to animate.

**Lifecycle.** Dialog and Popover emit `after-enter` and `after-leave`, and their default slot gets `phase` (`'entering' | 'entered' | 'leaving' | 'left'`). A component that's open on page load renders open, without animating.

### Your elements inside an overlay

Name your elements in `targets` and mark them with the slot's `target('name')` (Dialog, Drawer, BottomSheet, Popover) or with `v-animate:name` (any overlay). `after` times them against each other and against the component's own parts.

```vue
<script setup lang="ts">
import type { DialogMotion } from 'vael-ui'

const motion: DialogMotion<'summary' | 'rows'> = {
  targets: {
    summary: { initial: { opacity: 0, y: 8 } },
    rows: { initial: { opacity: 0, y: 6 }, stagger: 0.04, after: 'summary:60%' },
  },
}
</script>

<template>
  <Dialog v-model:open="open" title="Weekly report" :motion="motion">
    <template #default="{ target }">
      <p :ref="target('summary')">23 hours across 4 tasks.</p>
      <ul>
        <li v-for="row in rows" :key="row.name" :ref="target('rows')">{{ row.name }}</li>
      </ul>
    </template>
  </Dialog>
</template>
```

| `after` | Waits for |
| --- | --- |
| `'summary'` | it to arrive (default) |
| `'summary:start'` | it to start |
| `'summary:settle'` | its spring to come to rest |
| `'summary:40%'` | it to be 40% of the way in |
| `'panel'` | a part of the component |
| `['summary', 'chart']` | all of them |

`delay` shifts a target (negative overlaps). `steps: [{ to, after, delay, ...timing }]` moves a target through stops, holding at each until its `after` comes round. `select: 'li'` finds elements by selector instead of marking them. Closing never replays the chain: everything leaves together, quickly.

### `v-animate`

`v-animate` gives your own elements the same motion. Import `vAnimate` from `'vael-ui/motion'` (in a `<script setup vapor>` component, `vAnimateVapor as vAnimate` from the same place). The Nuxt module registers it for you.

**Names.** The word after the colon, as in `v-animate:rows`, is a name. An element on its own needs none. Name elements when they belong together: elements under one name that come in at the same time are one group, so `stagger` plays them one after another; inside a component (a Dialog), its `motion.targets.rows` sets how the elements named `rows` move, and they come and go with it; `after: 'rows'` lets another element wait for them; pages under one name can wait for each other with `swap: 'wait'`.

```vue
<script setup lang="ts">
import { vAnimate } from 'vael-ui/motion'
</script>

<template>
  <!-- A built-in entrance: in when it mounts, out the way it came when Vue removes it -->
  <article v-for="order in orders" :key="order.id" v-animate.rise>Order #{{ order.id }}</article>

  <!-- Modifiers pick, the value tunes: an entrance, a spring, a stagger, then your changes -->
  <li v-for="row in rows" :key="row.id" v-animate:rows.rise.stagger="{ initial: { y: 12 } }">…</li>

  <!-- Your own entrance -->
  <article v-animate="{ initial: { opacity: 0, x: -12 }, spring: 'smooth' }">…</article>
</template>
```

Modifiers: an entrance (`.fade`, `.rise`, `.drop`, `.pop`, `.blur`, or a preset of yours), a spring (`.snappy`, `.smooth`, `.bouncy`, `.gentle`), `.stagger`, and `.size`. The value's `initial` and `exit` merge over the entrance's; its timing replaces the entrance's. vue-tsc rejects an unknown modifier.

| Option | What it does |
| --- | --- |
| `initial` | where it comes in from |
| `exit` | where it leaves to; `false` leaves at once |
| `spring`, or `duration` + `easing` | its timing |
| `delay` | seconds, before its entrance and before a `layout` slide (it holds its old place meanwhile) |
| `stagger` | elements under the same `v-animate:name`, in page order: `true` (the 40 ms token), `'end'`, `'center'`, `'edges'`, seconds, or `(index, count) => seconds`. Across a grid, `'center'` and `'edges'` go by where each element sits: from the middle cell, or from the corners |
| `states` | looks held while hovered (a mouse), pressed (`active`), focused, or in view (`inView`): `{ hover: { scale: 1.03 }, inView: { opacity: 1 } }`. Each takes `onAfterEnter` / `onAfterLeave` (`(el) => …`, as it turns on and off), and `inView` its own `viewport` |
| `when: 'visible'` | waits until it scrolls into view, once |
| `viewport` | when it counts as in view, for `when: 'visible'` and `states.inView`: `{ amount: 'some' \| 'all' \| 0–1, margin: '0px 0px -20% 0px', once: true }` |
| `replay` | plays `initial` again whenever this value changes, and `animate`'s keyframes from the start (a line that redraws on each new period) |
| `swap: 'wait'` | replacing another element under the same name (a page, a wizard step): comes in once that one has left. Default `'sync'` crosses them |
| `onStart` | `(el) => …` as its entrance starts moving, after any `delay`, `after` or scroll into view |
| `onAfterEnter`, `onAfterLeave` | `(el) => …` once its entrance (or a `replay`) has played, and once its exit has played and it's gone |
| `onAfterMove` | `(el) => …` once a `layout` or `layoutId` slide, or a change of `animate`, has played |
| `enter`, `leave` | `(el, { signal, reducedMotion, index }) => …`: animate it yourself (GSAP, motion) instead of `initial` / `exit`; return the animation |
| `appear: false` | no entrance on mount; `replay` and exit still play |
| `appear: 'later'` | no entrance for what's there at the first render (inside a component's panel: when it opens); anything added after comes in |
| `after`, `steps` | wait on another name's moment (`'title'`, `'title:60%'`): inside a component, among elements that appear together, or for elements already on the page (what they're playing now) |
| `layout: true` | slides to its new place when the layout moves it, and eases to a new size; `layout: 'position'` only slides |
| `layoutId` | two elements with the same id are one element moving between places |
| `layout: 'position'` | with `layoutId`: travels without resizing, so text never stretches |
| `animate` | a look it holds; changes move from wherever it is on screen |
| `progress` | 0 to 1: holds `animate`'s keyframe arrays at that point, for scrubbing. A getter (`() => scroll.value`) is followed without re-rendering the component |
| `.size` modifier | eases the element to its content's new height, and width when the content sizes it |
| with `v-show` | hiding plays its exit on a copy where it was; showing plays its entrance again |
| `v-animate:name` | a name (see **Names** above): groups, joins the nearest component's `targets`, and is what `after` and `swap: 'wait'` refer to |
| a string | an entrance or a preset by name, for a name that changes: `v-animate="loading ? 'fade' : 'rise'"`. List your presets in `MotionPresets` (`import 'vael-ui/motion'`, then `declare module 'vael-ui/motion' { interface MotionPresets { wiggle: true } }`) and every name is checked. |

```vue
<!-- Grows to fit its content -->
<div v-animate.size class="panel">{{ expanded ? long : short }}</div>

<!-- Slides when the list reorders -->
<li v-for="task in tasks" :key="task.id" v-animate="{ layout: true }">{{ task.label }}</li>

<!-- A card that becomes a detail view -->
<button v-if="selected !== card" v-animate="{ layoutId: card.id }" @click="selected = card">…</button>
<div v-if="selected" v-animate="{ layoutId: selected.id, layout: 'position' }">…</div>

<!-- A held look: turns when `open` changes -->
<span v-animate="{ animate: { rotate: open ? 180 : 0 }, spring: 'snappy' }" class="chevron" />

<!-- A badge that pops each time the count changes -->
<span v-if="count" v-animate="{ initial: { scale: '1.3' }, spring: { damping: 0.6, response: 0.3 }, replay: count }">
  {{ count }}
</span>

<!-- A swipe scrubbing keyframes -->
<div v-animate="{ animate: { opacity: [0, 1], scale: ['0.9', '1'] }, progress: swipe }" />
```

Values beyond CSS: `rotateX`, `rotateY`, `skewX`, `skewY` and `perspective` (a card flip: `animate: { rotateY: flipped ? 180 : 0, perspective: 800 }`), and `pathLength` (0 to 1), `pathOffset`, `pathSpacing` to draw an SVG stroke (`initial: { pathLength: 0 }` draws a check in; the shape gets `pathLength="1"`).

Keyframe arrays: `null` first starts from where the value is now (`scale: [null, 1.2, 1]`). Beside `repeat`, `repeatType: 'loop' | 'reverse' | 'mirror'` (`mirror` comes back eased as it went) and `repeatDelay` (seconds held between runs).

Keyframes in `animate` with `repeat: Infinity` loop until the look changes:

```ts
const ring = {
  rotate: { value: [0, 14, -10, 6, -3, 0, 0], times: [0, 0.1, 0.25, 0.4, 0.55, 0.7, 1], easing: 'ease-in-out', duration: 1200, repeat: Infinity },
}
// <span v-animate="{ animate: ringing ? ring : { rotate: 0 } }">
```

On one element, a component's `:motion` wins over `v-animate`.

### From code

For motion an event starts (a wrong code shakes the field, an added item bumps the cart badge), `animate(target, values, options)` plays `v-animate`'s values and timing from code. The target is an element, a component, an array, or a selector. It goes from where the element is (or through your keyframes), and the end stays as the element's own style; a new call on the same element carries on from where the last one is. It follows the `ConfigProvider` spring, `motion: false` (jumps to the end) and reduced motion (only fades and colors animate).

```ts
import { animate, useAnimate } from 'vael-ui/motion'

const run = animate(field.value, { x: [0, -8, 8, -4, 0] }, { duration: 300 })
await run // finished; never for repeat: Infinity
run.pause(); run.play(); run.reverse(); run.stop(); run.finish()
run.time = 120; run.speed = 2

// Selectors inside an element of the component; runs stop on unmount.
const { animate } = useAnimate(useTemplateRef('row'))
animate('.badge', { scale: [1, 1.4, 1] }, { spring: 'bouncy' })
animate('.item', { opacity: [0, 1] }, { stagger: 0.05 })
```

Options: timing (`spring`, or `duration` + `easing`), `delay`, `stagger`, and for keyframes `times`, `repeat`, `repeatType`, `repeatDelay`, `onRepeat`. No sequence DSL: `await` orders, `Promise.all` runs together, `delay` overlaps. Prefer `states`, `animate` on `v-animate` or `replay` for motion that follows state; `animate()` is for events.

Dragging: the components that drag (BottomSheet, SwipeToReveal, Sortable, Slider) have it built in. To let people reorder your own list or move items between lists, use `v-draggable` with `useSortableGroup`. For an element dragged freely (VueUse's `useDraggable`), hold its offset in `animate` and set it back to 0 on release: `v-animate` springs it home.

### Tween a number

`useTween(source, options?)` is a number that eases to each new value of its source, from wherever it is: a total, a percentage, something you draw. It rides the app's spring unless you give a `spring`, or a `duration` and `easing`, and jumps with motion off or reduced motion. For a value that should keep its speed through a change (one following a drag), use `useSpringValue`.

```ts
import { useTween } from 'vael-ui/motion'

const shown = useTween(() => cart.total, { duration: 600, easing: 'out' })
```

### Pages

A route change is an element leaving and another coming in. Name the routed component; the old page leaves where it was as the new one comes in, or with `swap: 'wait'`, the new one comes in once the old one has gone.

```vue
<RouterView v-slot="{ Component, route }">
  <component
    :is="Component"
    :key="route.path"
    v-animate:page="{ initial: { opacity: 0, y: 12 }, exit: { opacity: 0, y: -12 }, swap: 'wait' }"
  />
</RouterView>
```

### Scroll

`useScrollProgress(target?, { container, axis, offset })` returns a ref from 0 to 1: the page's scroll, or `target`'s way through the screen (default `offset: ['start end', 'end start']`, from entering to having left; each pair is `'<target edge> <container edge>'` with `start`, `center`, `end`, a share, px or %). Pass it to `progress` as a getter so scrolling never re-renders the component:

```vue
<script setup lang="ts">
import { useTemplateRef } from 'vue'
import { useScrollProgress, vAnimate } from 'vael-ui/motion'

const panel = useTemplateRef<HTMLElement>('panel')
const progress = useScrollProgress(panel)
</script>

<template>
  <div ref="panel" v-animate="{ animate: { opacity: [0.3, 1, 0.3] }, progress: () => progress, spring: 'smooth' }" />
</template>
```

For "animate while visible", use `states.inView` with a `viewport`; for "play once on first sight", `when: 'visible'`.

### Hooks for GSAP or motion

`enter` and `leave` hand you the component's elements by part name. Return what your library returns: a GSAP tween or timeline, motion's `animate()`, a WAAPI `Animation`, or any promise. The component waits for it before it reports opened or removes the element.

```ts
import gsap from 'gsap'
import type { DialogMotion } from 'vael-ui'

const motion: DialogMotion<'rows'> = {
  targets: { rows: {} },
  enter: ({ panel }, { targets }) =>
    gsap
      .timeline({ defaults: { ease: 'power3.out' } })
      .from(panel, { opacity: 0, y: 16, duration: 0.3 })
      .from(targets.rows, { opacity: 0, x: -12, duration: 0.25, stagger: 0.05 }, '-=0.1'),
  leave: ({ panel }) => gsap.to(panel, { opacity: 0, duration: 0.12 }),
}
```

```ts
import { animate } from 'motion'
import type { PopoverMotion } from 'vael-ui'

const motion: PopoverMotion = {
  enter: ({ panel }) =>
    animate(panel, { opacity: [0, 1], scale: [0.96, 1] }, { type: 'spring', visualDuration: 0.25, bounce: 0 }),
  leave: ({ panel }) => animate(panel, { opacity: 0, scale: 0.98 }, { duration: 0.12 }),
}
```

List hooks get the item by its part name and its `index` among the items entering together:

```ts
const toasts: ToasterMotion = {
  enter: ({ toast }, { index }) => gsap.from(toast, { y: -16, opacity: 0, duration: 0.3, delay: index * 0.04 }),
  leave: ({ toast }) => gsap.to(toast, { x: 40, opacity: 0, duration: 0.15 }),
}
```

What the component guarantees:

- **Interruption.** Close mid-open or reopen mid-close and vael-ui stops your animation (`kill()`, `stop()` or `cancel()`) and aborts the context's `signal`. Start from the element's current state, not a fixed `from`, so a reversal doesn't jump (GSAP `to()` rather than `fromTo()`).
- **Return it.** A hook that starts an animation and returns nothing gets a dev warning, and the component won't wait.
- **Never stuck.** A hook that throws finishes at once; one that never ends is cut off after 3 s. Loops belong in CSS on a child.
- **Reduced motion.** Hooks are skipped for people who prefer reduced motion, and the built-in fade runs instead. Set `reducedMotion: 'run'` to run them anyway; the context's `reducedMotion` tells you, so you can drop movement yourself.
- **Context.** The second argument has `signal`, `origin` (the `transform-origin` it grows from), `reducedMotion`, `targets` (your marked elements by name) and `direction` (for swaps).

Overlays expose their elements (`panelEl`, `overlayEl`, `positionerEl`), and most other components expose `parts` (their elements by part name; repeated parts are arrays), for animation outside the hooks.

### Callbacks

They run once a motion has played, so the next step never guesses a duration. With nothing to play (motion off, an element off screen), they run at once. Never a `setTimeout` that repeats a duration.

| Where | Callbacks | They get |
| --- | --- | --- |
| An overlay's `motion` (Dialog, Drawer, Popover, …) | `onAfterEnter` once fully open, `onAfterLeave` once fully closed, as its `@after-enter` / `@after-leave` | its parts by name |
| A list's `motion` (Toaster, FileUpload, SpeedDial, DataTable, …) | `onAfterEnter`, `onAfterLeave`, per item | the item's parts, `{ index }` |
| A swap (Dialog, Popover, Calendar) | `onAfterSwap` once the new view is in | that view (Calendar: its grid), `{ direction }` |
| A state | `states.invalid.onAfterEnter` once on and played; `.onAfterLeave` once off and settled back | parts by name; a repeated part (OtpInput's `cell`) is an array |
| An always-on component's `initial` | `onAfterEnter` after its appearance, on mount and each `replay` | nothing |
| `v-animate`, or a name in `targets` | `onAfterEnter`, `onAfterLeave`; `v-animate` also `onAfterMove` | the element |
| A keyframe value | `onRepeat` beside `repeat`, as each repeat starts | how many so far (1, 2, …) |

```vue
<!-- Close once the success tick has drawn in -->
<svg v-if="saved" v-animate="{ initial: { scale: 0.25, opacity: 0 }, duration: 500, onAfterEnter: close }" />
```

Chain them so each step plays on a still page: the tick's `onAfterEnter` closes the dialog, and the dialog's `onAfterLeave` applies the result (adds the row, shows the toast). Applied while the dialog fades, the page moves under it, the exit reads as a cut, and the new row's entrance is hidden.

```ts
const pending = shallowRef<Visit | null>(null)
const DIALOG: DialogMotion = {
  onAfterLeave: () => {
    if (!pending.value) return
    visits.value = [pending.value, ...visits.value]
    toast.success('Visit saved')
    pending.value = null
  },
}
```

```ts
// Start over once the shake has played
states: {
  invalid: {
    root: { x: { value: [0, -8, 8, -5, 5, 0], duration: 400 } },
    onAfterEnter: ({ root }) => {
      code.value = ''
      invalid.value = false
      root.querySelector('input')?.focus()
    },
  },
}
// An overlay: select the field once open, toast once closed
const motion: DialogMotion = {
  onAfterEnter: ({ panel }) => panel.querySelector('input')?.select(),
  onAfterLeave: () => toast.success('Renamed'),
}
```

- A state that's already on when the component mounts doesn't run `onAfterEnter`; one that turns on while the motion code is still loading plays and does.
- `onAfterEnter` never runs for something that starts leaving first; its `onAfterLeave` does.
- Inside a component, an element's callbacks come from its `v-animate` value, else its `targets` entry. Prefer `targets` with `:ref="target('name')"`: both are typed, so a misspelled name is a type error.
- Your own animation on one element: `v-animate="{ enter: (el) => gsap.from(el, { y: 20, opacity: 0 }), leave: (el) => gsap.to(el, { opacity: 0 }) }"`. Return the tween; the callbacks wait for it, and it's stopped if the element leaves first.

### Indicators

Tabs, SelectButton, MenuList, Pagination and Toolbar slide an indicator.

```vue
<!-- Its own spring -->
<Tabs v-model:active="tab" :items="tabs" :motion="{ spring: { damping: 0.8, response: 0.3 } }" />

<!-- A pill that stretches as it travels (Tabs, Pagination, SelectButton) -->
<SelectButton v-model="view" :items="views" :motion="{ stretch: true, squash: 0.85 }" />
```

`stretch` takes `true` or `{ trail, delay }`. A `move` hook animates it yourself. It gets the indicator, already at its new place, and `from`, the offset back to where it was (`x`, `y`, `scaleX`, `scaleY`):

```ts
const motion: TabsMotion = {
  move: ({ indicator }, { from }) =>
    indicator.animate(
      [{ transform: `translate(${from.x}px, ${from.y}px)` }, { transform: 'none' }],
      { duration: 250, easing: 'cubic-bezier(0.23, 1, 0.32, 1)' },
    ),
}
```

For your own lists, `useHoverIndicator` and `useTabIndicator` give the same sliding highlight. See the [composable index](../composables/_INDEX.md).

### Your own engine

By default vael-ui plays motion with Web Animations. A style's `engine` hands overlay parts, list items and your targets to your library instead. It gets the element, `from` and `to` in vael-ui's names (`x`, `y`, `scale`, `opacity`, any CSS property; `blur` arrives as `filter`) and the timing in seconds, with the spring behind it. States, swaps and `linked` always use the built-in engine.

```ts
import { gsap } from 'gsap'
import { animate } from 'motion'
import type { MotionEngine } from 'vael-ui/motion'

export const gsapEngine: MotionEngine = (el, from, to, { duration, delay }) =>
  gsap.fromTo(el, from, { ...to, duration, delay, ease: 'power3.out' })

export const motionEngine: MotionEngine = (el, from, to, { duration, delay, spring }) =>
  animate(
    el,
    Object.fromEntries(Object.keys(to).map((key) => [key, [from[key], to[key]]])),
    spring
      ? { type: 'spring', visualDuration: spring.response, bounce: Math.max(0, 1 - (spring.damping ?? 1)), delay }
      : { duration, delay },
  )
```

```vue
<ConfigProvider :motion="{ version: 1, engine: gsapEngine }">
  <RouterView />
</ConfigProvider>
```

A component's own hooks still win over the engine.

### App-wide styles

A `MotionStyle` is one object on `ConfigProvider` that sets how the whole app moves. It needs `version: 1`.

```ts
import type { MotionStyle } from 'vael-ui/motion'

export const appMotion: MotionStyle = {
  version: 1,
  spring: { damping: 1, response: 0.32 },
  overlay: { initial: { opacity: 0, scale: '0.96' } },
  list: { stagger: 0.04 },
  press: { active: { scale: '0.97' } },
  content: { initial: { opacity: 0, y: 6 } },
  presets: {
    rise: { initial: { opacity: 0, y: 12 } },
    pop: { initial: { opacity: 0, scale: '0.9' }, spring: { damping: 0.7, response: 0.35 } },
  },
  toaster: { initial: { toast: { opacity: 0, y: -12 } } },
  input: { states: { invalid: { root: { x: [0, -6, 6, -3, 0] } } } },
}
```

```vue
<ConfigProvider :motion="appMotion">
  <RouterView />
  <!-- anywhere inside -->
  <article v-animate="'rise'">…</article>
</ConfigProvider>
```

- **Roles** take plain values, not parts: `overlay` (with `morph`, `surface`, `exitOrder`, `swap`, `resize`), `list` (with `stagger`, `leaving`, `appear`), `reveal`, `press` (`active`, `hover`) and `content` (your targets inside components).
  - `overlay`: Dialog, Drawer, BottomSheet, Popover, Menu, Tooltip, and the Select, Combobox, DatePicker and TreeSelect panels
  - `list`: Toaster, Timeline, FileUpload, SpeedDial, DataTable rows, CommandPalette rows, Breadcrumb, AvatarGroup
  - `press`: Button; `reveal`: Message and your own `useMotion('reveal')`
- **Component keys** (`dialog`, `toaster`, `select`, `button`, `switch`, …) take that component's motion shape, by part, and beat the role. `drawer` sits over `dialog`, so drawers can move apart from dialogs.
- **Presets** are your own `v-animate` entrances by name: `presets: { wiggle: {…} }` makes `v-animate.wiggle` (or `v-animate="'wiggle'"`) work inside the provider; a built-in name (`rise`) replaces that entrance there. Type the names with `import 'vael-ui/motion'` and `declare module 'vael-ui/motion' { interface MotionPresets { wiggle: true } }`, so they autocomplete and a typo fails vue-tsc.
- **Filters** name SVG filters the provider puts on the page: `filters: { goo: { blur: 8, contrast: [18, -7] } }`, used with `filter: url(#goo)`, such as on a Popover's `surface`.
- **Resolution:** built-in < role < component key < a nearer `ConfigProvider` < the component's own `motion`. A key replaces the one below it whole; `false` at any level stops everything below.
- **Data only.** A style can be JSON, except `engine` and hooks. `linked` is set on the component, never in a style.
- **From outside your code** (a settings page, a database), validate with `parseMotionStyle`. It drops unknown or unsafe keys and lists them in `issues`; it never throws.

```ts
import { parseMotionStyle } from 'vael-ui/motion'

const { style, issues } = parseMotionStyle(JSON.parse(saved))
```

`:motion="{ spring }"` without `version` is the short form: it only retimes. Add `version: 1` for a full style.

### `useMotion` for your own components

`useMotion(role, motion?)` gives your component the same motion as vael-ui's: it follows the app's style for that role, animates in and out, and your targets and `v-animate` children join it. Bind `transition` to Vue's `<Transition>` or `<TransitionGroup>`.

```vue
<script setup lang="ts">
import { useMotion } from 'vael-ui/motion'

const props = defineProps<{ open: boolean }>()
const motion = useMotion('overlay')
</script>

<template>
  <Transition v-bind="motion.transition">
    <div v-if="open" class="sheet">…</div>
  </Transition>
</template>
```

```vue
<script setup lang="ts">
import { useMotion } from 'vael-ui/motion'

const props = defineProps<{ rows: { id: string; label: string }[] }>()
const list = useMotion('list', () => ({ initial: { item: { opacity: 0, y: 6 } }, stagger: 0.04 }))
</script>

<template>
  <TransitionGroup tag="ul" v-bind="list.transition">
    <li v-for="row in rows" :key="row.id">{{ row.label }}</li>
  </TransitionGroup>
</template>
```

Roles: `'overlay'` (default), `'list'`, `'reveal'`, `'content'`. The second argument is a getter for that role's motion object, so pass a prop through as `() => props.motion`. It returns `transition`, `part(name)` (a function ref for one of your parts), `target(name)` (for elements inside), and the reactive `phase`. Type target names with `useMotion<'title' | 'lines'>()`. In development the root carries `data-motion`.

### Testing

```ts
import { settled } from 'vael-ui/motion'

await user.click(screen.getByRole('button', { name: 'Open' }))
await settled() // every animation vael-ui started has finished
await settled(field.value?.el) // only what's animating on that element
expect(screen.getByRole('dialog')).toBeVisible()
```

- Wrap the tree in `<ConfigProvider :motion="false">` for screenshots and end-to-end tests.
- In jsdom or happy-dom, which have no `element.animate`, motion finishes at once.
- A guard is a plain function; return `true` to pass straight through.

### Server rendering

- A `v-animate` entrance is written into the server HTML as CSS, so it plays before any script loads and doesn't replay on hydration. Content stays visible if JavaScript never loads.
- A component's `initial` never plays on hydration, and list `appear` never does either.
- An overlay that's open on load renders open, without animating.
- The runtime writes no styles before mount, so server and client markup match.
- Nuxt: the `vael-ui/nuxt` module registers `v-animate`. A `<script setup vapor>` component imports `vAnimate` from `'vael-ui/vapor'`.

### What never animates

vael-ui skips these on purpose:

- anything in a hidden tab
- a view restored by `KeepAlive`
- elements past the 24th appearing together, unless timed with `delay` or `stagger`: they just appear
- rows recycled while a virtual list scrolls
- keyboard navigation and search in Tree (a tree past 500 visible rows renders only the rows in view and still folds); DataTable changes over 150 rows

### Debugging

- An overlay, or a component using `useMotion`, carries `data-motion` on its root in development, naming where its motion came from.
- Each open and close shows as a measure in the DevTools Performance panel.
- A CSS `transition` on a property the motion animates gets a console warning. Remove it; the two fight.
- Slow it down to check feel: play it at 2 to 5 times its duration, or step it frame by frame in the DevTools Animations panel.

## Motion that feels right

The built-in motion already follows these rules. Apply them when you add or change motion. Decide in order, and stop as soon as the answer is "don't animate". Pick every value by what the motion does (a menu opening, a toast leaving, a text swap), never because a number looks close to another.

### 1. Should it animate at all?

| How often people see it | Decision |
| --- | --- |
| Hundreds of times a day: keyboard shortcuts, a command palette, typing | No motion. Keep it instant. |
| Tens of times a day: hover, list navigation, tabs | Built-in only, or quieter. Don't add. |
| Now and then: dialogs, drawers, toasts, menus | The built-in look, retimed or restyled |
| Rarely: onboarding, success, a five-star rating | Room for a flourish |

Anything triggered from the keyboard should feel instant. vael-ui already jumps the highlight on long keyboard moves and keeps typing unanimated.

```vue
<!-- Opened many times a day: keep the quiet built-in look, don't add stagger or bounce -->
<CommandPalette v-model:open="open" :items="commands" />
```

### 2. Name the purpose

Every animation needs one of these. If you can't name one, don't add it.

- **Feedback:** the interface heard you (a press, a shake on an invalid field)
- **Where it came from:** a panel grows from its trigger, a card becomes a dialog
- **What changed:** a toast that turns into an error, a chevron that turns
- **Smoothing a jump:** content that grows instead of snapping (`v-animate.size`, Message, AnimatedText)
- **Delight:** only for rare moments

Don't move data people are reading or acting on just for style: keep a chart, a table or a balance still.

### 3. Pick the tool

Use the first that fits, so the motion follows the app's spring, `motion: false` and reduced motion with no extra code:

| What moves | Use |
| --- | --- |
| A vael-ui component (a dialog, a menu, a list, a field) | its `motion` prop: values by part, a spring, `states` |
| Your element coming, going, moving or holding a look | `v-animate`: `initial` / `exit`, `layout`, `layoutId`, `animate`, `states` |
| Several of your elements in a sequence | names and `after` / `steps`, or `targets` on the component around them |
| Motion an event starts (a wrong code, an item added) | `animate()` or `useAnimate()` |
| A number on screen (a total, a percentage) | `useTween` (a duration) or `useSpringValue` (a spring that keeps its speed) |
| A timeline with labels, a library you already use | `enter` / `leave` hooks returning a GSAP or motion animation |
| Everything at once (the app's feel) | a `ConfigProvider` `motion` style |

Don't add a CSS `transition` or `@keyframes` to something vael-ui animates, and don't wrap its panels in `<Transition>`.

### 4. Enter quickly, leave quicker

- Enter and exit both ease **out**: they start fast, so the response feels immediate. Never ease **in** on UI; it delays the moment people are watching.
- Leaving is faster and quieter than entering: less distance, no bounce. vael-ui caps closes at about 200 ms, and an unset `exit` is already a quiet fade (a `v-animate` element leaves back the way it came, at half the distance). Inside a closing panel, content without its own `exit` simply leaves with the panel.
- Leave the way it came. A toast that drops in from the top leaves through the top. A drawer slides back to its edge.
- Don't delay a close or a hover-out. Dismissal must feel instant.
- Hover in is quick and direct; the way back may settle more softly, on a spring. Hover states only ever follow a mouse: `states.hover` ignores touch, so a tap never sticks a hover look.

```ts
const motion: DialogMotion = {
  initial: { panel: { opacity: 0, y: 12, scale: '0.96' } },
  exit: { panel: { opacity: 0, scale: '0.98' } }, // shorter and quieter than the way in
}
```

### 5. Durations, easing and springs

Use vael-ui's tokens before inventing numbers. They autocomplete in `duration` and `easing`.

| Token | Value | Use |
| --- | --- | --- |
| `--ui-duration-press` | 160 ms | press feedback |
| `--ui-duration-tooltip` / `-exit` | 125 / 100 ms | tooltips |
| `--ui-duration-enter` / `-exit` | 200 / 150 ms | panels, menus, dialogs |
| `--ui-duration-toast` / `-exit` | 400 / 200 ms | toasts |
| `--ui-duration-drawer` / `-exit` | 500 / 350 ms | drawers and sheets, which travel far |
| `--ui-duration-stagger` | 40 ms | between items |
| `--ui-ease-out` | `cubic-bezier(0.23, 1, 0.32, 1)` | entering and leaving (the default) |
| `--ui-ease-in-out` | `cubic-bezier(0.77, 0, 0.175, 1)` | moving something already on screen |
| `--ui-ease-drawer` | `cubic-bezier(0.32, 0.72, 0, 1)` | drawers and sheets |
| `--ui-ease-pop` | `cubic-bezier(0.34, 1.36, 0.64, 1)` | a small entrance with a little overshoot |

Values by what moves. Start here, and go quieter for anything frequent:

| What moves | Open | Close | Distance | Starting scale | Blur |
| --- | --- | --- | --- | --- | --- |
| Press feedback | 100–160 ms | at once | none | `0.97` (held while pressed) | none |
| Tooltip | 125 ms, after a short intent delay | 100 ms | 4 px | `0.98` | none |
| Menu, select, popover | 150–250 ms | 150 ms | 6–8 px | `0.97` | none |
| Dialog | 200–250 ms | 150 ms | 12–16 px | `0.96` | none |
| Drawer, sheet | 500 ms or a spring | 350 ms | its own size (`100%`) | none | none |
| Toast | 400 ms | 200 ms | its own height | `0.95` behind the front one | none |
| Text or icon swap | 150 ms, both ways | 150 ms | 4 px | none | 2 px |
| Page, step or tab panel | 200–250 ms | 120–150 ms | 8–12 px | none | 0–3 px |
| Error shake | 300–400 ms | — | 6–8 px a side | none | none |
| A rare flourish (success, a badge) | up to 500 ms, `'pop'` | 200 ms | up to 30 px | `0.9` | up to 8 px |

Rules:

- Keep UI motion under 300 ms. Only drawers, a dialog grown from its trigger and large travel go longer.
- Use `linear` only for constant motion (a progress sweep, a spinner) or a deliberate hold, like Button's `hold`.
- Built-in CSS keywords (`ease`, `ease-out`) are too weak for UI; use the tokens.

**Springs** are for movement, and for anything people can grab or interrupt: sheets, drags, indicators, a panel that reverses mid-way. Reach for these:

| Feel | Spring | Use |
| --- | --- | --- |
| vael-ui's default | `{ damping: 1, response: 0.32 }` | most motion |
| Snappy | `{ damping: 1, response: 0.25 }` | small, frequent changes |
| Smooth | `{ damping: 1, response: 0.4 }` | larger panels, repositioning |
| A touch of life | `{ damping: 0.8, response: 0.35 }` | after a flick, a drag release, a sheet |
| Bouncy | `{ damping: 0.55, response: 0.45 }` | rare, playful moments only |

Start at `damping: 1`. Add bounce only when the gesture carried momentum (a flick, a throw) or for a rare flourish. A menu that overshoots after a plain click feels wrong; a sheet that overshoots after a flick feels right. Never bounce a close.

### 6. Grow from where it came from

- Popover, Menu and Tooltip panels already grow from the exact center of their trigger, even after flipping to stay on screen. Don't override their `transform-origin`.
- Dialogs stay centered: they aren't anchored to a trigger. Use `origin="trigger"` or `morph` only when the dialog really is the trigger expanding (a card into its detail view).
- For your own floating elements, set the origin to the trigger, or use `morph: { from: () => el }`.

### 7. Make it interruptible

People change their minds mid-animation. Every vael-ui animation continues from where it is on screen when interrupted, so:

- Prefer held values and springs over one-shot keyframes for anything that can fire twice in a second. Keyframe arrays restart; held values retarget.
- In hooks, animate **to** a value from wherever the element is, and return the animation so vael-ui can stop it.
- Never block input while something animates. Stagger is decoration; people can click before it ends.

```ts
// Retargets from wherever the panel is if it reopens mid-close
leave: ({ panel }) => gsap.to(panel, { opacity: 0, y: 8, duration: 0.15 })
```

### 8. Never appear from nothing

Never animate from `scale: 0`. Start at `0.9` or higher, with opacity. Small surfaces barely scale (tooltips `0.98`), larger ones a little more (menus `0.97`, dialogs `0.96`). Below about `0.9` it reads as a zoom.

```ts
// Wrong: pops out of a point
initial: { panel: { scale: '0' } }
// Right
initial: { panel: { opacity: 0, scale: '0.96' } }
```

The same goes for distance: 4 px for a text swap, 6 to 8 px for list items and small panels, 12 to 16 px for a dialog. Anything over about 40 px, other than a drawer or sheet, feels slow.

### 9. Stagger and delay lightly

30 to 80 ms between items (`stagger: 0.04`). Keep the total under about 300 ms: for long lists, shrink the stagger or animate only the first rows. Past 24 items arriving together, vael-ui shows the rest at once unless you time them. In a grid, `stagger: 'center'` spreads from the middle cell, which reads better than page order.

A delay is right for three things only: a stagger, filtering an accidental trigger (a tooltip waits so a passing cursor doesn't open it), and sequencing on purpose (`after`). If something feels late, shorten its duration instead of adding a delay, and never delay a close.

### 10. Respect reduced motion

vael-ui handles it: under `prefers-reduced-motion`, built-in motion and your values keep their fades and drop movement and scale, and your hooks don't run. Don't fight it:

- Don't set `reducedMotion: 'run'` unless the hook already drops movement when `context.reducedMotion` is `true`.
- Reduced motion means gentler, not none. Keep fades that explain a change.
- Keep `:motion="false"` for a real "no animation" setting or tests.

```ts
const motion: DialogMotion = {
  reducedMotion: 'run',
  enter: ({ panel }, { reducedMotion }) =>
    reducedMotion
      ? gsap.from(panel, { opacity: 0, duration: 0.2 })
      : gsap.from(panel, { opacity: 0, y: 16, duration: 0.3, ease: 'power3.out' }),
}
```

### 11. Animate transform and opacity

- Move things with `x`, `y`, `scale`, `rotate` and `opacity`. These run on the compositor and stay smooth while the page is busy.
- Don't put `width`, `height`, `top`, `left`, `margin` or `padding` in values or states. For size, use `v-animate.size`, a component's own `resize`, or `layout`.
- Keep `blur` small (2 to 4 px) and short. It's expensive, especially in Safari. A little blur is right when two states cross (a text swap, a page sliding over another): it blends them into one change instead of two objects overlapping. Never on a plain fade.
- Don't add `will-change` yourself; vael-ui handles layers while it animates.
- Don't add a CSS `transition` to a part that `motion` animates; you'll get a warning and a fight.

### 12. Slow where people decide, fast where the system answers

A deliberate action can take time; the response never should. Button's `hold` fills over 1200 ms and snaps back at once on release. Use it instead of building your own hold-to-confirm.

```vue
<Button variant="danger" :hold="1500" @click="remove">Hold to delete</Button>
```

### 13. Match the mood, then check it

A dashboard or a form should be crisp: `damping: 1`, short distances. A playful product can bounce a little more. Set it once in a `ConfigProvider` style so every component agrees, rather than tuning each instance.

Check feel at 2 to 5 times the duration (DevTools' Animations panel) or frame by frame, test gestures on a real phone, and look again the next day. What to look for:

- Two states visibly overlapping in a crossfade (shorten it, or add 2 px of blur)
- A start or stop that jerks (wrong easing, or a spring with too little damping)
- Something growing from the wrong point (its origin)
- Properties out of step (an opacity that finishes long before the movement)
- A gap where nothing shows between one view leaving and the next arriving (the exit is too slow)
- Anything moving that people were reading

## Common mistakes

| Mistake | Fix |
| --- | --- |
| `motionCss`, `motion-css` | Renamed `motion` in 0.5.0 |
| `before-close="(done) => …"` to animate a close | `before-close` is a guard returning `boolean` or a promise. Animate with `exit` or a `leave` hook. |
| `@item-enter`, `@card-leave`, `@row-enter`, `@chip-leave`, `@action-enter` | Removed. Use `enter` / `leave` hooks in `motion` (`motion.items`, `motion.chips` for CommandPalette rows and chips). |
| `force-mount` to take over the animation | It only keeps content mounted. Use `motion` values or hooks. |
| Overriding a component's CSS `transition` with `!important` | Use `:motion`: values, a spring, or hooks. |
| `<AnimatePresence>` or a `<Transition>` wrapped around a vael-ui panel | The component owns presence. Use `motion` hooks, or `v-animate` inside. |
| A hook that starts a tween but returns nothing | Return it: `leave: ({ panel }) => gsap.to(panel, …)` |
| `gsap.fromTo()` from fixed values in a hook | `gsap.to()` / `from()`, so a reversal starts where the element is |
| `spring` together with `duration` | Pick one. |
| `delay: 200` meaning ms | `delay`, `stagger` and `response` are seconds: `delay: 0.2`. `duration` is ms. |
| `scale: 0` entrances | `opacity: 0, scale: '0.95'` |
| Animating `height` in a state | `v-animate.size`, `resize`, or `layout: true` |
| Bounce on a close, or on a plain click | `damping: 1` there; bounce only after momentum |
| Stagger on a command palette or anything frequent | Keep the built-in, quiet look |
| A list stagger that takes a second to finish | 0.03 to 0.05 s, total under about 300 ms |
| A shake that plays only once while the field stays invalid | Give the state a `replay` that changes on each attempt: `invalid: { root: { x: […] }, replay: attempts }` |
| A `MotionStyle` without `version: 1` | Add it; `{ spring }` alone is the retime-only short form |
| `linked` in a ConfigProvider style | Set it on the Dialog, Drawer or BottomSheet |
| `reducedMotion: 'run'` with full movement | Check `context.reducedMotion` and drop movement |
| Tests that wait with `setTimeout` | `await settled()`, or `:motion="false"` |
| Code that waits a fixed `setTimeout` for an animation to end | `onAfterEnter` / `onAfterLeave` on what animates: the element, the state, the overlay or the list |
| `v-animate` imported from `'vael-ui/vapor'` in a VDOM template | Import it from the build the template compiles as |

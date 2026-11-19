# Motion recipes

Start from the recipe, then adapt. Each one names the frequency tier, the purpose and the tool, then gives the code and the values, so two agents asked for the same motion produce the same motion.

Read [Motion](./motion.md) for the API and the rules behind these values. Every recipe passes the gate in SKILL.md: how often it's seen, what it's for, which tool, which properties, then curve and timing.

**Pick the tool in this order, and stop at the first that fits:**

1. The component already moves. Keep it, or retime it: `:motion="{ spring: 'gentle' }"`.
2. Your elements: `v-animate` (`initial`, `exit`, `states`, `layout`, `layoutId`).
3. Motion an event starts: `animate()` / `useAnimate()`.
4. Motion a number drives: a live value in `animate`, with `mapRange`.
5. A finger drives it: `useDrag`, `useWheel`, `usePanZoom`, `useDragSelect`.
6. Your own engine, only when the user asks for GSAP or motion: `enter` / `leave` hooks that return the animation.

Frequency tiers: **many times a day** (menus, tabs, command palettes, list navigation) keeps the built-in look or none; **occasional** (dialogs, drawers, toasts, settings) gets standard motion; **rare** (onboarding, success, celebration) is where delight belongs.

## Already built in

Before you animate it yourself, check whether a component plays it. These are the motion catalogs' usual requests, in vael-ui terms:

| The request | In vael-ui |
| --- | --- |
| A number that pops or counts | `AnimatedNumber`; `Badge`'s count changes on its own |
| A text swap, a label that changes | `AnimatedText` |
| An error shake, an invalid field | `Input` and the other fields: `:invalid`, or `states: { invalid: { root: { x: [0, -6, 6, -3, 0] } } }` |
| A skeleton that becomes content | `Skeleton :loading` around the real content |
| A sliding tab, segment or hover highlight | `Tabs`, `SelectButton`, `v-hover-indicator` on your own list |
| An accordion, a collapse | `Accordion`, `Collapsible` |
| A switch, a checkbox draw | `Switch`, `Checkbox` |
| A menu, a tooltip, a modal, a toast | `Menu`, `Tooltip`, `Dialog`, `toast()` |
| Hold to delete | `Button hold` |
| Avatars that spread on hover | `AvatarGroup` |

---

## Press and hover feedback

Many times a day. Feedback. Button and the other controls already press and hover; don't add motion to them. For your own pressable:

```vue
<div v-animate="{ states: { hover: { scale: 1.03 }, active: { scale: 0.97 } } }">…</div>
```

Press `0.95` to `0.98`, hover at most `1.03`. Never animate a keyboard-driven control beyond the built-in.

## Menu, select, popover, tooltip

Many times a day. Spatial consistency. Built in: the panel scales from its trigger (`0.97`, 6 to 8 px, 150 to 250 ms), and a tooltip is faster still, opening at once after the first. Don't wrap them in `<Transition>`. To make a popover grow out of its button, once per page:

```vue
<Popover v-model:open="open" open-on-trigger-click :motion="{ morph: 'trigger' }">…</Popover>
```

## Dialog, drawer, bottom sheet

Occasional. Spatial consistency. Built in (Dialog `0.96` and 12 to 16 px, 200 to 250 ms; Drawer and BottomSheet on a spring). Retime or retune, never rebuild:

```vue
<Drawer v-model:open="open" :motion="{ spring: 'gentle' }" />
<Dialog :motion="{ initial: { panel: { opacity: 0, y: 16, scale: '0.96' } } }" />
```

A close is faster than the open and never bounces. A dialog stays centered: use `morph` only when it really is the trigger expanding.

## Toast

Occasional. Feedback. `toast('Saved')` enters and leaves through the same edge, and a swipe along that edge dismisses it. Toasts with an `action` stay until dismissed. Don't set `duration` unless you mean to.

## A list that comes in

Occasional. Preventing a jarring change. One group, one after another, total under about 300 ms:

```vue
<li v-for="row in rows" :key="row.id" v-animate:rows.rise.stagger>…</li>
```

Lead first and furthest, support follows with less, decoration fades only. One entrance per container: if a panel animates in, its rows are already there. Never stagger a list people open from the keyboard.

## A list that changes

Occasional. Spatial consistency. The rest close the gap while the removed row fades:

```vue
<li v-for="task in tasks" :key="task.id" v-animate:tasks.rise="{ layout: true }">…</li>
```

When something opens above a list, give the list `layout: 'position'` too, or it snaps.

## Scroll reveal

Rare. Preventing a jarring change. Plays once on first sight, and a number sweep follows the scroll with no render:

```vue
<section v-animate.rise="{ when: 'visible' }">…</section>
<div v-animate="{ animate: { opacity: [0.3, 1, 0.3] }, progress: () => progress, spring: 'smooth' }" />
```

```ts
const progress = useScrollProgress(useTemplateRef('panel')) // 0 to 1 through the screen
```

## A page change

Occasional. The old page leaves as the new one comes in:

```vue
<component :is="Component" :key="route.path" v-animate:page="{ initial: { opacity: 0, y: 8 }, swap: 'wait' }" />
```

## A shared element (lightbox, card to detail)

Occasional. Spatial consistency. Two elements with the same id are one element moving between places. Clipped trips and rotation are handled:

```vue
<button v-if="!open" v-animate="{ layoutId: photo.id }" @click="open = true">…</button>
<div v-if="open" v-animate="{ layoutId: photo.id, layout: 'position' }">…</div>
```

## Success, built in steps

Rare. State indication. One sequence, each step after the one before:

```ts
const { animate } = useAnimate(useTemplateRef('stage'))
await animate([
  ['.card', { opacity: [0, 1], scale: [0.92, 1] }, { name: 'card' }],
  ['.check path', { pathLength: [0, 1] }, { at: 'card:60%', duration: 300 }],
  ['.title', { opacity: [0, 1], y: [6, 0] }, { at: '-0.15' }],
])
```

Start the second move just before the first ends, so it reads as one gesture. Under reduced motion it jumps to the end.

## Hold to confirm

Occasional. Feedback. Slow where people decide, instant on release:

```vue
<Button variant="danger" :hold="1500" @click="remove">Hold to delete</Button>
```

## A number that counts

Rare to occasional. `AnimatedNumber` for a displayed value, `useTween(() => total, { duration: 600, easing: 'out' })` for one you draw.

## Swipe to dismiss

Occasional. Feedback and spatial consistency. It leaves along the axis it came in on, fades as you drag, and keeps your speed:

```ts
const drag = useDrag({
  el: notice,
  axis: 'x',
  onRelease({ projected }) {
    if (Math.abs(projected.x) < width() / 3) return null // home, on a spring
    gone = Math.sign(projected.x)
    shown.value = false
  },
})
```

```vue
<div v-if="shown" ref="notice" v-animate="{
  animate: { opacity: () => mapRange(Math.abs(drag.x.value), [0, width()], [1, 0.2]) },
  exit: () => ({ x: { value: gone * width() * 1.2, spring: 'smooth' }, opacity: 0 }),
}" />
```

Give the exit's `x` a spring, or the card stops at the edge and only then fades.

## A card deck

Occasional or rare. The top card follows the finger and tilts, the cards behind rise toward it, and a thrown card keeps its speed. Each card's look is a live value of the one number the drag writes:

```vue
<article v-for="(card, i) in cards" :key="card.id" :ref="(el) => claim(el, card)" v-animate="{
  animate: {
    rotate: () => (i === 0 ? mapRange(drag.x.value, [-width(), width()], [-12, 12]) : 0),
    scale: { value: () => mapRange(slot(i), [0, 3], [1, 0.82]), spring: true },
    opacity: { value: () => mapRange(slot(i), [2, 3], [1, 0]), spring: true },
  },
  exit: () => ({ x: { value: gone * far(), spring: 'smooth' }, rotate: gone * 25, opacity: 0 }),
}" />
```

`slot(i)` is the card's place in the stack, `i - Math.min(1, Math.abs(drag.x.value) / width())`. Add Like and Pass buttons: a drag has no keyboard form.

## A draggable floating panel

Occasional. A dialog moved by its header, throwing and stopping at the window's edge:

```ts
const panel = computed(() => dialog.value?.panelEl ?? null)
const drag = useDrag({ el: panel, handle: '.drag-header', onRelease: ({ projected }) => inWindow(projected) })
```

Give the handle `touch-action: none`, and a grip button that moves it with the arrow keys through `drag.throwTo`.

## A wheel picker, a carousel

Occasional. State indication. It rests on one item; you drag, throw, scroll or step it:

```ts
const index = shallowRef(0)
const wheel = useWheel(index, { el: useTemplateRef('el'), count: places.length, size: 170, orientation: 'horizontal', loop: true, onTick: haptic.selection })
```

Place each item with `translate: var(--ui-wheel-distance, 200vw) 0`, and style it with `v-animate="{ animate: look, progress: () => wheel.progressOf(i) }"`. Previous and Next call `wheel.step(∓1)`, `@keydown="wheel.onKeydown"` on the element.

## Pan and zoom

Occasional. A photo, a map or a diagram. Drag, pinch, double tap, ctrl and wheel, and keys:

```ts
const pz = usePanZoom({ el: useTemplateRef('box'), maxScale: 5 })
```

`@keydown="pz.onKeydown"` on the box, `pz.zoomBy(1.5)` on the buttons. `pz.scale` is live: fade labels in with `mapRange(pz.scale.value, [at, at + 0.6], [0, 1])`. A slider that drives the zoom calls `pz.zoomTo(v, { animate: false })`, or its thumb lags the spring and jitters.

## Rubber-band selection

Occasional. Feedback. Drag across empty space; the box's children are the items:

```ts
const sel = useDragSelect({ el: useTemplateRef('box'), spring: 'bouncy' })
```

Draw the band inside the box, `position: absolute`, `pointer-events: none`, with live `x`, `y`, `width` and `height`. `sel.stretch` is how far the corner trails the pointer, to bow the band's sides. Shift adds, ctrl or cmd toggles, Escape restores. A touch presses and holds first, then drags.

## Before you hand it over

- It has a purpose you can name in a word, and it isn't on a keyboard or hundreds-a-day path.
- Ease out both ways, leave faster than you enter, under 300 ms (dialogs, drawers, sheets and toasts excepted).
- Nothing scales from `0`; nothing animates `width`, `height`, `top` or `left`.
- It survives a second trigger mid-flight, and a gesture can be caught.
- Reduced motion keeps fades and drops movement (it's done for you; check hooks).
- You can name what to feel-check: record it, step it frame by frame at 2 to 5 times the duration, try the gesture on a phone.

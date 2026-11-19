---
name: vael-motion
description: Build, change or review motion in a Vue app that uses vael-ui. Decides whether something should move at all, then picks the cheapest tool (a component's `motion` prop, `v-animate`, `animate()`, a live value, or a gesture composable such as `useDrag`, `useWheel`, `usePanZoom`, `useDragSelect`) and the values. Use when asked to animate something, add or retime motion, stagger a list, make a page or panel transition, build a swipe, drag, carousel, picker or zoom, or wire GSAP or motion into a vael-ui component. Pairs with the `vael-ui` skill, which owns setup, components and styling.
license: MIT
metadata:
  author: Mini-Sylar
  version: 1.0.0
---

# vael-ui motion

One job: turn a request for motion into code that plays on vael-ui's motion system and would pass a strict review the first time. It does not install the library or pick components (that's `vael-ui`), and it doesn't write motion for apps without vael-ui (that's `animate`).

Two failure modes, and the first is worse:

1. **Animating what shouldn't move.** The gate below exists to produce zero lines sometimes. That's a success.
2. **Fighting what's built in.** A `<Transition>` around a vael-ui panel, a CSS `transition` on a part vael-ui animates, a hand-written drag. vael-ui already has the motion; retime it, don't replace it.

Make the call, state the reasoning in one line, write the code. Don't present motion options as a menu.

## Hard rules

1. **Gate first.** Frequency and purpose (Step 1) before any tool or value.
2. **Built in first.** Open the component's reference file, read its Motion section, and retime with `:motion` before you replace anything.
3. **No fighting.** No `<Transition>`, `AnimatePresence`, `!important` or `transition` on a part vael-ui animates, and no `will-change`.
4. **Never hand-roll a drag.** Free drag is `useDrag`, a picker or carousel `useWheel`, pan and zoom `usePanZoom`, band selection `useDragSelect`, reordering `Sortable` or `v-draggable`.
5. **A number drives a look through a live value** (`animate: { rotate: () => … }`), never `el.style.transform`.
6. **Values come from the tables here or the guide.** Never invent a curve, a spring or a duration.
7. **Reduced motion and `motion: false` are handled.** Don't add your own checks. In tests, `await settled()`.
8. **The project's taste wins.** If the repo has a `DESIGN.md` (or a CLAUDE.md / AGENTS.md section) with Motion rules, they beat these defaults. When the user corrects a value, follow it and say it's worth recording with `/learn-taste`.

## Step 1: Should it move, and why?

| How often it's seen | Decision |
| --- | --- |
| Hundreds a day, or from the keyboard (command palette, menus, tabs, list navigation) | Keep the built-in look or none. **No stagger. Stop here.** |
| Tens a day (hover, toggles) | Near-imperceptible: fast and small, or nothing |
| Occasional (dialogs, drawers, toasts, settings) | Standard motion |
| Rare (onboarding, success, celebration) | Delight is allowed here |

Then name the purpose in one word: **feedback**, **where it came from** (spatial consistency), **what changed** (state), **smoothing a jump**, **explanation** (onboarding only), **delight** (rare tier only). If you can't, don't build it. Data people are reading or acting on doesn't move for style.

## Step 2: Cast and relate, when more than one thing moves

- **Cast:** one lead (moves first and furthest), support (follows with less), decoration (fades only), everything else still. Most groups shrink here.
- **Relate:** together by default. Overlap (a stagger) only when order carries meaning: 30 to 80 ms apart, total under about 300 ms, unequal gaps. Sequence (`after`, `animate([…])`) only when overlap would look wrong.
- **One entrance per container.** A panel that animates in carries its content. Never stagger a list people open from the keyboard.
- **Exits go together**, shorter and simpler than the entrance, through the way they came. Never stagger a close.

## Step 3: Pick the tool

Walk down and stop at the first that fits.

| The motion is | Tool |
| --- | --- |
| A vael-ui component's own | its `motion` prop: `false`, a spring, values by part, `states`, hooks |
| Your element coming, going, moving or holding a look | `v-animate`: `initial` / `exit`, `layout`, `layoutId`, `states`, `animate` |
| Started by an event (a wrong code, a sent payment) | `animate()` / `useAnimate()`, or a list of steps for a sequence |
| Following a number (a drag, a scroll, the pointer, a slider) | a live value in `animate`, with `mapRange`; `progress` for a keyframe sweep |
| A number you display or draw | `AnimatedNumber`, or `useTween` |
| Driven by a finger | `useDrag`, `useWheel`, `usePanZoom`, `useDragSelect` |
| The user's own library (GSAP, motion) | `enter` / `leave` hooks that **return** the animation |

```vue
<Drawer v-model:open="open" :motion="{ spring: 'gentle' }" />                      <!-- same look, other feel -->
<Dialog :motion="{ initial: { panel: { opacity: 0, y: 16, scale: '0.96' } } }" />  <!-- values by part -->
<li v-for="r in rows" :key="r.id" v-animate:rows.rise.stagger="{ layout: true }" />  <!-- a group -->
<article v-animate="{ animate: { rotate: () => mapRange(drag.x.value, [-300, 300], [-20, 20]) } }" />
```

Imports: components, their motion types and the gesture composables from `vael-ui`; everything for your own elements (`vAnimate`, `animate`, `useAnimate`, `mapRange`, `useTween`, `settled`, `useSpringValue`) from `vael-ui/motion`. Inside a component use `useAnimate()`; plain `animate` is for helpers and tests that already hold an element. Name an element (`v-animate:rows`) only when something else must point at it: a stagger group, `after`, or a component's `targets`.

## Step 4: Values

| What | Enter | Leave |
| --- | --- | --- |
| Tooltip | about 125 ms, `scale: '0.98'` | faster |
| Menu, select | 150 to 250 ms, `scale: '0.97'`, 6 to 8 px | faster |
| Dialog | 200 to 250 ms, `scale: '0.96'`, 12 to 16 px | faster |
| Drawer, sheet, anything grabbable | a spring: `'snappy'` or `'smooth'`; `'bouncy'` only after a flick | never bounces |

Units: `duration` in ms; `delay`, `stagger` and `spring.response` in seconds. A spring and a duration never go together. Entrances by name: `fade`, `rise`, `drop`, `pop`, `blur`.

Ease out both ways, never ease in. Leave faster than you enter, under 300 ms. Never scale from 0 (`'0.95'` or more, with `opacity: 0`). Delay only to stagger, to filter an accidental trigger, or to sequence; never delay a close. Animate `x`, `y`, `scale`, `rotate`, `opacity`; never `width`, `height`, `top` (use `.size`, `resize`, `layout`). A page swap, sliding indicator, accordion or icon swap keeps the same timing both ways.

## Step 5: A finger drives it

While the pointer is down the element is the finger: 1:1, no easing, no spring, no `transition`, no render per move (read `x`, `y`, `scale`, `position` in getters). On release a spring keeps the speed. Decide from `projected` (where the throw would land) so a short flick counts. Bounce only after a throw, never for a button or key. A new press catches it mid-flight. Everything linked to the drag reads the one number it writes. A drag has no keyboard form: add `throwTo`, `step` and `onKeydown`. Give a thrown element's `exit` a spring (`x: { value: far, spring: 'smooth' }`), or it stops at the edge and only then fades.

## Where to read

Don't read the whole guide. Open the section the task needs.

| The task | Read |
| --- | --- |
| A starting point for a list, page change, shared element, success, swipe, deck, picker, zoom | [Recipes](../vael-ui/references/guides/recipes.md) |
| Retime or retune a component | [Motion guide](../vael-ui/references/guides/motion.md#the-api-by-task): turn it off, spring, values, states, lists, overlays |
| Your own elements | [`v-animate`](../vael-ui/references/guides/motion.md#v-animate), [in order](../vael-ui/references/guides/motion.md#in-order), [scroll](../vael-ui/references/guides/motion.md#scroll), [live values](../vael-ui/references/guides/motion.md#live-values) |
| A drag, a picker, pan and zoom | [Gestures](../vael-ui/references/guides/motion.md#gestures), then the composable's reference in [composables](../vael-ui/references/composables/_INDEX.md) |
| GSAP or motion | [Hooks](../vael-ui/references/guides/motion.md#hooks-for-gsap-or-motion) |
| Why a value is what it is | [Motion that feels right](../vael-ui/references/guides/motion.md#motion-that-feels-right) |
| Something is wrong | [Common mistakes](../vael-ui/references/guides/motion.md#common-mistakes) |

## Never ship

| Never | Instead |
| --- | --- |
| `<Transition>` / `AnimatePresence` around a vael-ui panel | `:motion` |
| `transition: all`, `ease-in`, `scale(0)` | named properties, ease out, `'0.95'` with `opacity: 0` |
| A stagger on a menu, a command palette, tabs or any keyboard path | the built-in look, or none |
| Motion on something hundreds-a-day with no purpose you can name | nothing |
| `pointermove` or VueUse `useDraggable` plus a CSS settle, a `setInterval` carousel | the gesture composables |
| A spring or `transition` on an element while it's dragged | nothing: tracking is 1:1 |
| A swipe decided from the offset alone, or an `exit` with a plain number after a throw | `projected`; `x: { value, spring: 'smooth' }` |
| `el.style.transform` for something a getter can drive | a live value |
| `setTimeout(duration)` to act after motion | `onAfterEnter` / `onAfterLeave`; `settled()` in tests |
| A hook that starts a tween and returns nothing | return it: `leave: ({ panel }) => gsap.to(…)` |

## Output

Write the code. Then, in a few lines:

- **The gate:** the frequency tier and the named purpose. If part of the request was declined (a stagger on a menu), say which and why.
- **The ingredients:** tool, values and timing, one line each.
- **What to feel-check,** when the result depends on feel: play it at 2 to 5 times the duration, step it frame by frame, interrupt it mid-flight, try a gesture on a real phone, look again tomorrow.

When the honest answer is "this shouldn't move", give it. That answer is the reason this skill exists.

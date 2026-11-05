# useAnimate

Plays motion from your code, for something an event starts: a wrong code shaking, an item flying to the cart, a success card building in. `animate(target, values, options)` takes the same values and timing as `v-animate`, follows the app's spring, `motion: false` and reduced motion, and leaves the end as the element's own style. Give it a list of steps instead and it plays them one after another as one sequence. `useAnimate()` is the same `animate` scoped to a component: selectors match inside its element, and whatever it started stops when the component unmounts. For motion that plays when elements come in, use `v-animate` with `after` and `stagger` in the template instead.

```ts
import { animate, useAnimate } from 'vael-ui/motion' // either build
```

## Example

```ts
import { useTemplateRef } from 'vue'
import { useAnimate } from 'vael-ui/motion'

const { animate } = useAnimate(useTemplateRef('card'))

// One element: shake it.
function wrong() {
  animate('.field', { x: [0, -8, 8, -4, 0] }, { duration: 300 })
}

// A sequence: each step after the one before, unless its `at` says otherwise.
async function sent() {
  await animate([
    ['.card', { opacity: [0, 1], scale: [0.92, 1] }, { name: 'card' }],
    ['.check path', { pathLength: [0, 1] }, { at: 'card:60%' }],
    ['.title', { opacity: [0, 1], y: [6, 0] }, { at: '-0.15' }],
  ])
}
```

## Parameters

Name | Type | Description
--- | --- | ---
`element` | `Readonly<ShallowRef<Element \| ComponentPublicInstance \| null>>` | `useAnimate` only: the element selectors match inside. Leave it out and bind the returned `scope` with `ref="scope"`.
`target` | `Element \| ComponentPublicInstance \| string \| ArrayLike<Element>` | What moves: an element, a component, a list of elements, or a selector (inside the scope, with `useAnimate`).
`values` | `MotionValues` | Where it moves to: `x`, `y`, `scale`, `rotate`, `opacity`, `pathLength`, any CSS property. An array is keyframes, from the first to the last. From wherever it is when there is no first.
`options.spring / duration / easing` | `SpringName \| Spring / number / string` | Its timing: a spring by name or numbers, or a `duration` in ms and an `easing`. Unset, the app's spring.
`options.delay` | `number` | Seconds before it starts.
`options.stagger` | `boolean \| number \| 'end' \| 'center' \| 'edges' \| 'random'` | With several elements, one after another: `true`, seconds between each, or an order.
`options.times / repeat / repeatType / repeatDelay` | `number[] / number / 'loop' \| 'reverse' \| 'mirror' / number` | Keyframes: where each falls (0 to 1), how many more times they play (`Infinity` loops), how they repeat and the seconds held between runs.
`steps` | `SequenceStep[]` | Instead of a target: a sequence, each step `[target, values, options]`. A step takes the options above plus `at` and `name`.
`step options.at` | `SequenceAt` | Where the step starts. Unset: when the step before ends. `'+0.2'` / `'-0.15'`: seconds after or before that. `'50%'`: halfway through the step before. `1.2`: seconds from the start. `'card'`: with the step named `card`; `'card:60%'`: 60% of the way through it. TypeScript suggests the names your steps declare.
`step options.name` | `string` | A name for the step, for a later step's `at`.

## Returns

Name | Type | Description
--- | --- | ---
`controls` | `AnimationControls` | Await it for the end (never for an endless loop), or steer it: `pause()`, `play()`, `reverse()`, `finish()`, `stop()`, and `time` (ms) and `speed` to read or set. A sequence is steered as one.
`scope` | `ShallowRef<Element \| null>` | `useAnimate` only: the scope element, the one you passed or one to bind.
`animate` | `typeof animate` | `useAnimate` only: `animate`, scoped to the element and stopped on unmount.


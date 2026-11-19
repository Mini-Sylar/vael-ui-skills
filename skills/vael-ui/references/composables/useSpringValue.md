# useSpringValue

vael-ui's spring engine: the physics behind Sortable, Tree and DataTable reordering, and every sliding indicator set to a spring. A spring has no duration. You give it a target, and it moves there from where it is, at the speed it already has. So every animation is interruptible: call `set()` mid-flight and the value turns toward the new target with no jump. One shared `requestAnimationFrame` loop runs every moving spring and stops once they all rest.

```ts
import { useSpringValue, createSpring, stepSpring } from 'vael-ui/motion' // either build
```

## Which one?

- `useSpringValue`: use it when the template reads the value. It returns a ref.
- `createSpring`: use it when the value writes straight to an element, as in a gesture. It calls `onFrame` each frame, so nothing re-renders.
- `stepSpring`: use it in tests and plots. It is the pure step under both.

## Parameters

Name | Type | Description
--- | --- | ---
`initial` | `number` | The starting value. The spring rests there until you call `set()`.
`options.damping` | `number` | How much the spring resists bouncing. Default `1`: the value reaches the target with no overshoot. Below `1` it overshoots and oscillates, more the lower you go. Above `1` it creeps in more slowly. Save overshoot for motion you threw, such as a flick release, not for something that only appeared.
`options.response` | `number` | How quickly the spring converges, in seconds. Default `0.35`. It is the period of the undamped oscillation, not how long the spring runs: the real settle time comes from the distance, the speed and the damping. Lower is snappier.
`options.epsilon` | `number` | When the spring stops. Once the distance to the target and the speed per second are both below it, the spring snaps to the target. Default `0.05`, which suits px. Lower it for a value on a 0 to 1 scale, such as opacity, where 0.05 is a visible step.
`onFrame` | `(value: number) => void` | Receives the new value on every frame, on `jump()`, and on a `set()` that is already at rest on the target. Write the value straight to a style here. `createSpring` only.

## Returns

Name | Type | Description
--- | --- | ---
`value` | `Ref<number>` | The live value, updated every frame. Read it, and move it with `set()` and `jump()` instead of assigning to it. `useSpringValue` only.
`set(target, { velocity? })` | `(target: number, options?: { velocity?: number }) => void` | Sets a new target, starting from the current value and velocity, so an interrupted spring turns smoothly. Pass `velocity`, in units per second, to continue a gesture: the animation starts at the speed the drag ended with, so there is no seam.
`jump(value)` | `(value: number) => void` | Moves to `value` at once, with zero velocity. Use it for the first placement, while a drag follows the pointer, and under reduced motion.
`stop()` | `() => void` | Stops the spring where it is now. The current value becomes the target.
`createSpring(initial, options?, onFrame?)` | `SpringHandle` | Creates the same spring without a ref. Besides `set`, `jump` and `stop`, the handle has read-only `value`, `velocity`, `target` and `isAnimating`, and `destroy()`, which takes it off the frame loop. Call `destroy()` when you unmount; `useSpringValue` does that for you when its scope ends. Options are read once, so create a new spring to change them.
`stepSpring(state, target, dt, damping?, response?)` | `SpringState` | Runs one semi-implicit Euler step from `{ value, velocity }` over `dt` seconds. It's pure, with no frame loop, so you can test or plot a spring with plain numbers. A `dt` of 0 or less returns the state unchanged. The live loop caps each frame at 1/30 s, so a tab returning from the background doesn't fling every spring.


# useSpringValue

vael-ui's spring engine: the physics behind Sortable, Tree and DataTable reordering, and behind every sliding indicator set to spring mode. A spring has no duration. You give it a target, and it moves there from where it is now, at the speed it already has. That makes every animation interruptible: call `set()` mid-flight and the value turns toward the new target with no jump. `useSpringValue` gives you a ref to bind in a template. `createSpring` writes each frame through a callback instead, so a value that drives a style never re-renders a component. `stepSpring` is the pure integrator step under both, for tests and plots. One shared `requestAnimationFrame` loop drives every moving spring, and it stops scheduling frames once they all rest.

```ts
import { useSpringValue, createSpring, stepSpring } from 'vael-ui/motion' // either build
```

## Parameters

Name | Type | Description
--- | --- | ---
`initial` | `number` | The starting value. The spring rests there until you call `set()`.
`options.damping` | `number` | Default `1`, critically damped: the value reaches the target with no overshoot. Below `1` it overshoots and oscillates, more the lower you go. Above `1` it creeps in more slowly. Save overshoot for motion the user threw, such as a flick release, not for something that only appeared.
`options.response` | `number` | Default `0.35`, in seconds. Sets how quickly the spring converges, not how long it runs: it is the period of the undamped oscillation. The real settle time comes out of the physics, from the distance, the speed and the damping. Lower is snappier.
`options.epsilon` | `number` | Default `0.05`. Once the distance to the target and the speed per second are both below this, the spring lands on the target and stops. The default suits px. Lower it for a value on a 0 to 1 scale, such as opacity, where 0.05 is a visible step.
`onFrame` | `(value: number) => void` | `createSpring` only. Called with the new value on every frame, on `jump()`, and on a `set()` that is already at rest on the target. Write the value straight to a style here.

## Returns

Name | Type | Description
--- | --- | ---
`value` | `Ref<number>` | `useSpringValue` only. The live value, updated every frame. Read it, and drive it with `set()` and `jump()` rather than assigning to it.
`set(target, { velocity? })` | `(target: number, options?: { velocity?: number }) => void` | Retargets from the current value and velocity, never from the previous target, so an interrupted spring turns smoothly. Pass `velocity`, in units per second, to hand off a gesture's release speed: the animation starts at the speed the drag ended with, so there is no seam.
`jump(value)` | `(value: number) => void` | Moves to `value` at once, with zero velocity. Use it for the first placement, while a drag follows the pointer, and under reduced motion.
`stop()` | `() => void` | Parks the spring where it is now and drops the remaining motion. The current value becomes the target.
`createSpring(initial, options?, onFrame?)` | `SpringHandle` | The same spring without a ref. Besides `set`, `jump` and `stop`, the handle has read-only `value`, `velocity`, `target` and `isAnimating`, and `destroy()`, which takes it off the frame loop. Call `destroy()` when you unmount. `useSpringValue` does that for you when its scope ends. Options are read once, so create a new spring to change them.
`stepSpring(state, target, dt, damping?, response?)` | `SpringState` | One semi-implicit Euler step from `{ value, velocity }` over `dt` seconds. Pure, with no frame loop, so you can test or plot a spring with plain numbers. A `dt` of 0 or less returns the state unchanged. The live loop caps each frame at 1/30 s, so a tab returning from the background doesn't fling every spring.

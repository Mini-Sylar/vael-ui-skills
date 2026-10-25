# Bring your own animation

This guide moved into the [Motion guide](./motion.md) in vael-ui 0.5.0, when every component moved onto one motion system. Start there.

Where to look for each way of taking over:

| You want to | Use | Motion guide section |
| --- | --- | --- |
| Turn a component's animation off | `:motion="false"` | [Turn it off](./motion.md#turn-it-off) |
| Keep the look, change the feel | `:motion="{ spring: { damping, response } }"` | [Retime with a spring](./motion.md#retime-with-a-spring) |
| Change how one part comes in or leaves | `initial` / `exit` by part | [Change a part's values](./motion.md#change-a-parts-values) |
| Run GSAP, motion or WAAPI on the real elements | `enter` / `leave` hooks that return the animation | [Hooks for GSAP or motion](./motion.md#hooks-for-gsap-or-motion) |
| Run every component through your library | a style's `engine` | [Your own engine](./motion.md#your-own-engine) |
| Animate list items coming and going | `initial`, `exit`, `stagger`, `leaving`, or hooks | [Lists](./motion.md#lists) |
| Confirm before an overlay closes | `before-close` returning `boolean` or a promise | [Overlays](./motion.md#overlays) |
| Animate your own elements | `v-animate`, `useMotion` | [`v-animate`](./motion.md#v-animate), [`useMotion`](./motion.md#usemotion-for-your-own-components) |

## Coming from 0.4

The 0.4 hand-off API is gone. Code written against it breaks:

- `motionCss` / `motion-css` is now `motion`.
- `before-close` is a guard. `(done) => …` no longer runs, and an exit animation there never plays. Return `true` to close, `false` to stay open, or a promise of either. Animate the close with a `leave` hook:

  ```ts
  // 0.4
  beforeClose: (done) => gsap.to(panel, { opacity: 0, onComplete: done })
  // 0.5
  motion: { leave: ({ panel }) => gsap.to(panel, { opacity: 0 }) }
  ```

- `force-mount` no longer means "I'll animate it". It keeps content mounted so its state survives a close. To animate force-mounted content, add `motion` values or hooks.
- List events are removed: `item-enter` / `item-leave` (CommandPalette, FileUpload, Timeline), `card-enter` / `card-leave` (Toaster), `row-enter` / `row-leave` (DataTable), `chip-enter` / `chip-leave` (Select, Combobox), `action-enter` / `action-leave` (SpeedDial). Use `enter` / `leave` hooks in `motion`; return the animation instead of calling `done()`. CommandPalette rows go under `motion.items`, chips under `motion.chips`.
- Don't wrap a vael-ui panel in motion-v's `<AnimatePresence>` or your own `<Transition>`. The component owns its presence; use hooks, or `v-animate` on elements inside.

The [changelog](./changelog.md) lists every 0.5.0 change.

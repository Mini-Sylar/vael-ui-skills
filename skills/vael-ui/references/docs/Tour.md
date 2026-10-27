# Tour

A guided walkthrough that spotlights one element at a time behind a positioned callout, for onboarding and product tours.

```ts
import { Tour } from 'vael-ui' // or 'vael-ui/vapor'
```

## Props

Name | Type | Default | Description
--- | --- | --- | ---
`steps` | `readonly T[]` |  | Steps to walk through, in order: `{ target, title, description }`, where `target` is the element (or a selector) the step points at. A step can also set its callout's `side`, `align` and spotlight, and a `group` for "Section 2 of 3".
`id` | `string \| undefined` |  | Identifies this tour instance (see `useTour`'s `id` option).
`modal` | `boolean \| undefined` | `true` | Scroll-locks the page and makes everything but the target and callout inert while open.
`closeOnEsc` | `boolean \| undefined` | `true` | Escape key closes the tour, firing `@skip`.
`closeOnOverlay` | `boolean \| undefined` | `false` | Clicking the dimmed area closes the tour.
`keyboardNav` | `boolean \| undefined` | `true` | `ArrowLeft` and `ArrowRight` move back and forward a step, swapped right to left.
`spotlightPadding` | `number \| undefined` | `4` | Space between the target and the spotlight cutout, in pixels. Per-step `spotlightPadding` wins.
`spotlightRadius` | `number \| undefined` | `8` | Spotlight cutout corner radius, in pixels. Per-step `spotlightRadius` wins.
`scrollIntoView` | `boolean \| undefined` | `true` | Scrolls the target into view on every step change.
`teleportTo` | `string \| HTMLElement \| undefined` |  | Teleport target: a CSS selector or element. Wins over `container` either way.
`container` | `DOMTarget \| undefined` |  | Scopes the tour to an element. The spotlight dims only that box and the callout teleports there. Scroll lock and inert apply only inside it. Omit it for a page-level tour.
`scrollTarget` | `DOMTarget \| undefined` |  | Element to scroll-lock while open. Defaults to `container`, then `document.body`.
`forceMount` | `boolean \| undefined` | `false` | Keeps it mounted while closed, toggled with `v-show`, so its content keeps its state. vael-ui then leaves its motion to you.
`beforeClose` | `BeforeClose<PopoverCloseReason> \| undefined` |  | Runs before the tour closes. Return `true` to close it, `false` to keep it open, or a promise of either, like `() => confirmAction(…).result`. The tour stays open while the promise is pending.
`motion` | `TourMotion \| undefined` | `true` | How the callout moves, like Popover's, and whether the spotlight glides between steps. `false` turns both off, so you can animate `spotlightEl` yourself. An object moves the callout your way: values for the `panel` part, `{ spring }`, or `enter` / `leave` hooks. Its `{ duration, easing }` or `{ spring }` also times the glide between steps. Unset, the glide is quick to a nearby target and longer across the screen.
`ui` | `Partial<{ spotlight: UiPartValue; positioner: UiPartValue; panel: UiPartValue; }> \| undefined` |  | Class and style overrides for each part.
`open` | `boolean \| undefined` | `false` | Whether the tour is open.
`step` | `number \| undefined` | `0` | Current step index; resets to the first step whenever the tour opens.

## Slots

Name | Type | Description
--- | --- | ---
`default` | `{ id: string \| undefined; step: T \| undefined; index: number; total: number; group: string \| undefined; groups: TourGroup<T>[]; isFirst: boolean; isLast: boolean; isTransitioning: boolean; next: () => Promise<void>; prev: () => Promise<void>; skip: () => void; close: () => void; panelEl: HTMLElement \| null; }` | Replaces the callout content; receives the current `step`, progress and `next`/`prev`/`skip`/`close`.

## Events

Name | Type | Description
--- | --- | ---
`open-change` | `[value: boolean, details: PopoverOpenChangeDetails]` | Fires when a close is requested; call `details.cancel()` to keep the tour open.
`skip` | `[details: TourEndDetails<T>]` | Fires when the tour ends early: the Skip button, Escape, or the step's target disappearing.
`step-change` | `[details: TourStepChangeDetails<T>]` | Fires after moving to a step, including the first. The new step renders by the next frame.
`finish` | `[details: TourEndDetails<T>]` | Fires when the tour advances past the last step.
`update:open` | `[value: boolean]` | Fires when `open` changes (`v-model:open`).
`update:step` | `[value: number]` | Fires when `step` changes (`v-model:step`).

## Exposed

Name | Type | Description
--- | --- | ---
`id` | `() => string \| undefined` | Returns the tour's `id` prop.
`targetEl` | `HTMLElement \| null` | Current step's target element.
`panelEl` | `HTMLElement \| null` | Callout panel element (`null` while closed).
`positionerEl` | `HTMLElement \| null` | Positioning wrapper around the callout panel (`null` while closed).
`spotlightEl` | `HTMLElement \| null` | Dimmed overlay with the cutout (`null` while closed). It carries the cutout's box, kept current through the glide: `--ui-tour-cutout-x`, `-y`, `-width`, `-height` and `-radius`, in px.
`isClosing` | `boolean` | `true` while a `beforeClose` close is pending.
`close` | `() => void \| undefined` | Closes the tour, running `@open-change` and `beforeClose` first.
`cancelClose` | `() => void \| undefined` | Cancels a close pending in `beforeClose` and keeps the tour open.
`currentIndex` | `number` | Current step index.
`isTransitioning` | `boolean` | `true` while a step's `onBeforeEnter` is pending; the previous step stays visible until it settles.
`next` | `() => Promise<void>` | Goes to the next step, or finishes the tour on the last one.
`prev` | `() => Promise<void>` | Goes back one step.
`skip` | `() => void` | Closes the tour and fires `@skip`.

## Motion

`motion` takes `TourMotion` (exported from `'vael-ui'`); `false` turns it off. Tour's `motion`: the callout opens and closes like Popover's. Its timing, `{ duration, easing }` or `{ spring }`, also sets how the spotlight and the callout glide between steps. Unset, the glide is quick to a nearby target and longer across the screen. `false` turns it all off.

- Parts: `positioner`, `panel`
- Keys: `spring`, `duration`, `easing`, `initial`, `exit`, `enter`, `leave`, `targets`, `morph`, `exitOrder`, `resize`, `surface`, `onAfterEnter`, `onAfterLeave`, `swap`, `direction`, `onAfterSwap`

See the [Motion guide](../guides/motion.md) for what each key does.


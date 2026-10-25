# useTour

The headless state machine behind `<Tour>`. It tracks the step index and groups, handles navigation and awaits each step's `onBeforeEnter`. It renders nothing and doesn't touch the DOM. `<Tour>` is `useTour()` plus a spotlight overlay and a `Popover` callout. Use the composable directly to build your own walkthrough UI. For example, you might use a different animation library, a non-floating callout or an embedded panel.

```ts
import { useTour } from 'vael-ui' // or 'vael-ui/vapor'
```

## Example

```ts
import { ref } from 'vue'
import { useTour } from 'vael-ui'
import type { TourStep } from 'vael-ui'

const open = ref(false)
const steps: TourStep[] = [
  { target: '#new-doc', title: 'Create something new' },
  { target: '#share', title: 'Invite your team' },
]

const { currentStep, currentIndex, total, isFirst, isLast, isTransitioning, next, prev, skip } =
  useTour(open, {
    steps,
    onFinish: () => console.log('tour finished'),
  })

// Point your own spotlight/callout at currentStep.value.target; next()/prev()/skip()
// drive it, isTransitioning tells you when an onBeforeEnter is still pending.
open.value = true
```

## Parameters

Name | Type | Description
--- | --- | ---
`open` | `Ref<boolean>` | Controls the tour's visibility. Setting it to `true` resets to the first step: it awaits that step's `onBeforeEnter`, then fires `onStepChange` with `reason: 'open'`. Setting it to `false` closes the tour without resetting it.
`id` | `string` | Identifies this tour instance. It isn't used internally. It's passed back in every callback's `details`, so a shared handler can tell tours apart when a page has more than one.
`steps` | `MaybeRefOrGetter<readonly TourStep[]>` | Same shape as `<Tour>`'s `steps` prop. Each step has a `target` (a `DOMTarget`), plus optional `title`, `description`, `side`, `align`, `sideOffset`, `alignOffset`, `spotlightPadding`, `spotlightRadius`, `disableInteraction`, `onBeforeEnter` and `group`.
`onStepChange` | `(details: TourStepChangeDetails) => void` | Fires after a step change settles, including the first step (`reason: 'open'`). `details` is `{ index, step, reason, previousIndex, previousStep, id }`.
`onSkip` | `(details: TourEndDetails) => void` | Fires when you call `skip()`. `details` is `{ index, step, id }`.
`onFinish` | `(details: TourEndDetails) => void` | Fires when you call `next()` on the last step. `details` is `{ index, step, id }`.

## Returns

Name | Type | Description
--- | --- | ---
`id` | `string \| undefined` | The `options.id` you passed in.
`currentIndex` | `Ref<number>` | 
`currentStep` | `ComputedRef<TourStep \| undefined>` | 
`currentGroup` | `ComputedRef<string \| undefined>` | 
`groups` | `ComputedRef<TourGroup[]>` | `{ group, steps }[]`, grouped in first-seen order.
`total / isFirst / isLast` | `ComputedRef` | 
`isTransitioning` | `Ref<boolean>` | `true` while the next step's `onBeforeEnter` is pending. `currentIndex` doesn't change until it settles, so keep the previous step's UI mounted until then.
`next / prev` | `() => Promise<void>` | On the last step, `next()` calls `onFinish` and sets `open.value = false` instead of advancing.
`skip` | `() => void` | Calls `onSkip` and sets `open.value = false`.
`goTo` | `(index: number) => Promise<void>` | 
`goToGroup` | `(group: string) => Promise<void>` | Jumps to the group's first step.

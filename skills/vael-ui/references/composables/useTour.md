# useTour

The headless state machine behind `<Tour>` — index/group bookkeeping, step navigation, and the `onBeforeEnter`-await sequencing, with zero DOM or rendering baked in. `<Tour>` is just `useTour()` plus a spotlight overlay and a composed `Popover` callout; reach for this directly to build a fully custom walkthrough UI (a different animation library, a non-floating callout, an embedded panel) and keep only the sequencing logic.

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
`open` | `Ref<boolean>` | Owns the tour's visibility. Setting it true resets to the first step (awaits that step's onBeforeEnter, then fires onStepChange with reason: 'open'); setting it false is just "closed," no reset happens on its own.
`id` | `string` | Identifies this tour instance. Not used internally — echoed back on every callback's details, useful once a page has more than one tour and a shared handler needs to tell them apart.
`steps` | `MaybeRefOrGetter<readonly TourStep[]>` | Same shape as `<Tour>`'s own steps prop: target (a DOMTarget) plus title/description/side/align/spotlightPadding/spotlightRadius/disableInteraction/onBeforeEnter/group per step.
`onStepChange` | `(details: TourStepChangeDetails) => void` | Fires after a step change settles, including the first step (reason: 'open'). details: { index, step, reason, previousIndex, previousStep, id }.
`onSkip` | `(details: TourEndDetails) => void` | Fires when skip() is called. details: { index, step, id }.
`onFinish` | `(details: TourEndDetails) => void` | Fires when next() is called on the last step. details: { index, step, id }.

## Returns

Name | Type | Description
--- | --- | ---
`id` | `string \| undefined` | Echoed straight back from options.id.
`currentIndex` | `Ref<number>` | 
`currentStep` | `ComputedRef<TourStep \| undefined>` | 
`currentGroup` | `ComputedRef<string \| undefined>` | 
`groups` | `ComputedRef<TourGroup[]>` | { group, steps }[], bucketed in first-seen order.
`total / isFirst / isLast` | `ComputedRef` | 
`isTransitioning` | `Ref<boolean>` | True while the current step's onBeforeEnter is pending — keep the previous step's UI mounted until this clears.
`next / prev` | `() => Promise<void>` | next() on the last step calls onFinish and sets open.value = false instead of advancing.
`skip` | `() => void` | Fires onSkip and sets open.value = false.
`goTo` | `(index: number) => Promise<void>` | 
`goToGroup` | `(group: string) => Promise<void>` | Jumps to that group's first step.


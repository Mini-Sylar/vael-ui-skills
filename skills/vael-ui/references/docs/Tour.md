# Tour

A guided walkthrough that spotlights one element at a time behind a positioned callout, for onboarding and product tours.

```ts
import { Tour } from 'vael-ui' // or 'vael-ui/vapor'
```

## Props

Name | Type | Default | Description
--- | --- | --- | ---
`steps` | `readonly T[]` |  | 
`id` | `string \| undefined` |  | Identifies this tour instance — see `useTour`'s `id` option.
`modal` | `boolean \| undefined` | true | Scroll-locks the page and makes everything but the target and callout inert while open.
`closeOnEsc` | `boolean \| undefined` | true | Escape key closes the tour.
`closeOnOverlay` | `boolean \| undefined` | false | Clicking the dimmed area closes the tour.
`keyboardNav` | `boolean \| undefined` | true | ArrowLeft/ArrowRight step back/forward.
`spotlightPadding` | `number \| undefined` | 4 | Space between the target and the spotlight cutout, in pixels. Per-step `spotlightPadding` wins.
`spotlightRadius` | `number \| undefined` | 8 | Spotlight cutout corner radius, in pixels. Per-step `spotlightRadius` wins.
`scrollIntoView` | `boolean \| undefined` | true | Scrolls the target into view on every step change.
`teleportTo` | `string \| HTMLElement \| undefined` |  | CSS selector or an actual DOM element — same contract as Vue's own Teleport `to`. Wins over `container` either way.
`container` | `DOMTarget \| undefined` |  | Scopes the tour to one element: the spotlight dims only its own box, the callout teleports there instead of `body`, and scroll-lock/inert apply only inside it — the rest of the page stays interactive. Omit for a page-level tour.
`scrollTarget` | `DOMTarget \| undefined` |  | Element whose scrolling is locked while open. Defaults to `container`, then `document.body`.
`forceMount` | `boolean \| undefined` | false | 
`beforeClose` | `((done: () => void) => void) \| undefined` |  | 
`ui` | `Partial<{ spotlight: UiPartValue; positioner: UiPartValue; panel: UiPartValue; }> \| undefined` |  | 
`open` | `boolean \| undefined` | false | 
`step` | `number \| undefined` | 0 | 

## Slots

Name | Type | Description
--- | --- | ---
`default` | `{ id: string \| undefined; step: T \| undefined; index: number; total: number; group: string \| undefined; groups: TourGroup<T>[]; isFirst: boolean; isLast: boolean; isTransitioning: boolean; next: () => Promise<void>; prev: () => Promise<void>; skip: () => void; close: () => void; panelEl: HTMLElement \| null; }` | 

## Events

Name | Type | Description
--- | --- | ---
`open-change` | `[value: boolean, details: PopoverOpenChangeDetails]` | 
`step-change` | `[details: TourStepChangeDetails<T>]` | 
`skip` | `[details: TourEndDetails<T>]` | 
`finish` | `[details: TourEndDetails<T>]` | 
`update:open` | `[value: boolean]` | 
`update:step` | `[value: number]` | 

## Exposed

Name | Type | Description
--- | --- | ---
`id` | `unknown` | Type inference unavailable — vue-component-meta cannot resolve defineExpose on this generic component.
`targetEl` | `unknown` | Type inference unavailable — vue-component-meta cannot resolve defineExpose on this generic component.
`panelEl` | `unknown` | Type inference unavailable — vue-component-meta cannot resolve defineExpose on this generic component.
`isClosing` | `unknown` | Type inference unavailable — vue-component-meta cannot resolve defineExpose on this generic component.
`close` | `unknown` | Type inference unavailable — vue-component-meta cannot resolve defineExpose on this generic component.
`cancelClose` | `unknown` | Type inference unavailable — vue-component-meta cannot resolve defineExpose on this generic component.
`currentIndex` | `unknown` | Type inference unavailable — vue-component-meta cannot resolve defineExpose on this generic component.
`isTransitioning` | `unknown` | Type inference unavailable — vue-component-meta cannot resolve defineExpose on this generic component.
`next` | `unknown` | Type inference unavailable — vue-component-meta cannot resolve defineExpose on this generic component.
`prev` | `unknown` | Type inference unavailable — vue-component-meta cannot resolve defineExpose on this generic component.
`skip` | `unknown` | Type inference unavailable — vue-component-meta cannot resolve defineExpose on this generic component.


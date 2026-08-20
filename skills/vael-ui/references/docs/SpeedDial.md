# SpeedDial

A floating action button that fans out into secondary actions on demand.

```ts
import { SpeedDial } from 'vael-ui' // or 'vael-ui/vapor'
```

## Props

Name | Type | Default | Description
--- | --- | --- | ---
`items` | `readonly T[]` |  | 
`direction` | `SpeedDialDirection \| undefined` | "up" | 
`openOn` | `SpeedDialTriggerMode \| undefined` | "click" | `hover` only activates on real hover-capable pointers; click always works too.
`disabled` | `boolean \| undefined` | false | 
`closeOnSelect` | `boolean \| undefined` | true | Selecting an action closes the dial; `false` keeps it open.
`ariaLabel` | `string \| undefined` | "Actions" | Accessible name for both the trigger button and the action `role="menu"`.
`radius` | `number \| undefined` | 96 | Arc radius (px) for `direction="quarter-circle"` — ignored otherwise.
`motionCss` | `boolean \| undefined` | true | Gates the built-in action fan-out/fan-in transition. `false` skips it entirely — reach for `@action-enter`/`@action-leave` instead if you want a consumer-owned animation (a spring, a staggered GSAP timeline) in its place.
`ui` | `Partial<{ root: UiPartValue; trigger: UiPartValue; action: UiPartValue; }> \| undefined` |  | 
`open` | `boolean \| undefined` | false | 

## Slots

Name | Type | Description
--- | --- | ---
`icon` | `{ open: boolean; }` | 
`item` | `{ item: T; index: number; }` | 

## Events

Name | Type | Description
--- | --- | ---
`select` | `[item: T]` | 
`action-enter` | `[el: Element, done: () => void]` | 
`action-leave` | `[el: Element, done: () => void]` | 
`update:open` | `[value: boolean]` | 

## Exposed

Name | Type | Description
--- | --- | ---
`el` | `unknown` | Type inference unavailable — vue-component-meta cannot resolve defineExpose on this generic component.
`listEl` | `unknown` | Type inference unavailable — vue-component-meta cannot resolve defineExpose on this generic component.
`open` | `unknown` | Type inference unavailable — vue-component-meta cannot resolve defineExpose on this generic component.
`close` | `unknown` | Type inference unavailable — vue-component-meta cannot resolve defineExpose on this generic component.
`toggle` | `unknown` | Type inference unavailable — vue-component-meta cannot resolve defineExpose on this generic component.


# SpeedDial

A floating action button that fans out into secondary actions on demand.

```ts
import { SpeedDial } from 'vael-ui' // or 'vael-ui/vapor'
```

## Props

Name | Type | Default | Description
--- | --- | --- | ---
`items` | `readonly T[]` |  | Actions to fan out, one icon button each.
`direction` | `SpeedDialDirection \| undefined` | `'up'` | Which way the actions fan out from the trigger. `'quarter-circle'` lays them on an arc of `radius`.
`openOn` | `SpeedDialTriggerMode \| undefined` | `'click'` | What opens the dial. `'hover'` only responds to hover-capable pointers; click always works too.
`disabled` | `boolean \| undefined` | `false` | Disables the trigger and blocks interaction.
`closeOnSelect` | `boolean \| undefined` | `true` | Closes the dial when you select an action; `false` keeps it open.
`ariaLabel` | `string \| undefined` | `'Actions'` | Accessible name for both the trigger button and the action `role="menu"`.
`radius` | `number \| undefined` | `96` | Arc radius in pixels. Only applies when `direction` is `'quarter-circle'`.
`motionCss` | `boolean \| undefined` | `true` | Plays the built-in fan-out and fan-in transition. Set `false` to animate actions yourself via `@action-enter` and `@action-leave`.
`ui` | `Partial<{ root: UiPartValue; trigger: UiPartValue; action: UiPartValue; }> \| undefined` |  | Class and style overrides for each part.
`open` | `boolean \| undefined` | `false` | Whether the actions are shown.

## Slots

Name | Type | Description
--- | --- | ---
`icon` | `{ open: boolean; }` | Replaces the trigger's plus icon.
`item` | `{ item: T; index: number; }` | Custom content for each action button. Unset, the button shows the item's `icon`.

## Events

Name | Type | Description
--- | --- | ---
`select` | `[item: T]` | Fires when you click an action.
`action-enter` | `[el: Element, done: () => void]` | Fires when an action's fan-out transition starts. Call `done()` when finished. Only fires when `motionCss` is `false`.
`action-leave` | `[el: Element, done: () => void]` | Fires when an action's fan-in transition starts, as with `@action-enter`.
`update:open` | `[value: boolean]` | Fires when `open` changes (`v-model:open`).

## Exposed

Name | Type | Description
--- | --- | ---
`el` | `HTMLElement \| null` | Root element.
`listEl` | `HTMLElement \| null` | Action list element (`role="menu"`).
`open` | `() => void` | Shows the actions. No-op while `disabled`.
`close` | `() => void` | Hides the actions.
`toggle` | `() => void` | Shows or hides the actions. No-op while `disabled`.


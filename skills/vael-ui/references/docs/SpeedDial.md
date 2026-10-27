# SpeedDial

A floating action button that fans out into secondary actions on demand.

```ts
import { SpeedDial } from 'vael-ui' // or 'vael-ui/vapor'
```

## Props

Name | Type | Default | Description
--- | --- | --- | ---
`items` | `readonly T[]` |  | Actions to fan out, one icon button each.
`direction` | `'right' \| 'left' \| 'up' \| 'down' \| 'quarter-circle' \| undefined` | `'up'` | Which way the actions fan out from the trigger, on screen: `'left'` is left on a right-to-left page too. `'quarter-circle'` lays them on an arc of `radius`.
`openOn` | `'hover' \| 'click' \| undefined` | `'click'` | What opens the dial. `'hover'` only responds to hover-capable pointers; click always works too.
`disabled` | `boolean \| undefined` | `false` | Disables the trigger and blocks interaction.
`closeOnSelect` | `boolean \| undefined` | `true` | Closes the dial when you select an action; `false` keeps it open.
`ariaLabel` | `string \| undefined` | `'Actions'` | Accessible name for both the trigger button and the action `role="menu"`.
`radius` | `number \| undefined` | `96` | Arc radius in pixels. Only applies when `direction` is `'quarter-circle'`.
`motion` | `SpeedDialMotion \| undefined` | `true` | How the actions fan out and back, and the icon turns. `false` shows them and turns the icon at once. An object animates the actions your way: values for the `action` part, `stagger`, or `enter` / `leave` hooks for your own library.
`ui` | `Partial<{ root: UiPartValue; trigger: UiPartValue; action: UiPartValue; icon: UiPartValue; }> \| undefined` |  | Class and style overrides for each part.
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
`update:open` | `[value: boolean]` | Fires when `open` changes (`v-model:open`).

## Exposed

Name | Type | Description
--- | --- | ---
`el` | `HTMLElement \| null` | Root element.
`listEl` | `HTMLElement \| null` | Action list element (`role="menu"`).
`iconEl` | `HTMLElement \| null` | Trigger icon wrapper. Turns 45deg while open.
`open` | `() => void` | Shows the actions. No-op while `disabled`.
`close` | `() => void` | Hides the actions.
`toggle` | `() => void` | Shows or hides the actions. No-op while `disabled`.

## Motion

`motion` takes `SpeedDialMotion` (exported from `'vael-ui'`); `false` turns it off. SpeedDial's actions: values or hooks for `action`, the list's `stagger` and `leaving`.

- Parts: `action`
- Keys: `spring`, `duration`, `easing`, `initial`, `exit`, `enter`, `leave`, `stagger`, `leaving`, `appear`, `onAfterEnter`, `onAfterLeave`

See the [Motion guide](../guides/motion.md) for what each key does.


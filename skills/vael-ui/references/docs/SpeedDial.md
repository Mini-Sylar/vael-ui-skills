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
`update:open` | `[value: boolean]` | 

## Exposed

_None._


# Menu

A dropdown list of actions opened from a trigger, with full keyboard navigation.

```ts
import { Menu } from 'vael-ui' // or 'vael-ui/vapor'
```

## Props

Name | Type | Default | Description
--- | --- | --- | ---
`items` | `readonly MenuEntry<T>[] \| undefined` |  | Data-driven rows. Ignored when the default slot renders custom markup instead.
`triggerEl` | `TriggerRef` |  | External ref for a trigger that can't live in the #trigger slot.
`side` | `Side \| undefined` | "bottom" | Which side of the trigger the panel opens on.
`align` | `Align \| undefined` | "start" | How the panel aligns against the trigger along that side.
`sideOffset` | `number \| undefined` | 8 | Gap between the trigger and the panel, in pixels.
`alignOffset` | `number \| undefined` | 0 | Shifts the panel along the alignment axis, in pixels.
`closeOnEsc` | `boolean \| undefined` | true | Escape key closes the panel.
`closeOnOutside` | `boolean \| undefined` | true | Clicking outside the panel closes it.
`beforeClose` | `((done: () => void) => void) \| undefined` |  | Custom exit animation; call `done()` when it's complete. Delays the actual close/unmount until then.
`forceMount` | `boolean \| undefined` | false | When true, presence is v-show-driven and owned by the consumer (e.g. AnimatePresence).
`teleportTo` | `string \| HTMLElement \| undefined` | "body" | CSS selector or an actual DOM element — same contract as Vue's own Teleport `to`.
`scrollFade` | `boolean \| undefined` | true | Masks the panel's top/bottom edge as its content scrolls under it, signaling there's more.
`ui` | `Partial<{ positioner: UiPartValue; panel: UiPartValue; }> \| undefined` |  | Per-instance part-class/style overrides.
`open` | `boolean \| undefined` | false | Whether the menu is open.

## Slots

Name | Type | Description
--- | --- | ---
`trigger` | `{ open: boolean; }` | Co-located trigger markup — Menu wires the click and anchors to it; render just the button.
`item` | `{ item: T; }` | Override one data-driven row's content while keeping its behavior.
`default` | `{ close: () => void; open: boolean; isClosing: boolean; cancelClose: () => void; panelEl: HTMLElement \| null; placement: string; }` | Fully custom menu content — render your own role="menuitem" markup; `items` is ignored.

## Events

Name | Type | Description
--- | --- | ---
`open-change` | `[value: boolean, details: PopoverOpenChangeDetails]` | 
`select` | `[item: T]` | 
`collapse` | `[]` | 
`active` | `[value: boolean]` | 
`update:open` | `[value: boolean]` | 

## Exposed

_None._


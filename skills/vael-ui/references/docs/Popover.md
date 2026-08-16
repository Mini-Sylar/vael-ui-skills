# Popover

A floating panel anchored to a trigger element, for menus, forms, or extra detail.

```ts
import { Popover } from 'vael-ui' // or 'vael-ui/vapor'
```

## Props

Name | Type | Default | Description
--- | --- | --- | ---
`triggerEl` | `TriggerRef` |  | External ref (raw element or component with `.el`); use #trigger slot if the trigger can live here.
`side` | `Side \| undefined` | "bottom" | Which side of the trigger the panel opens on.
`align` | `Align \| undefined` | "center" | How the panel aligns against the trigger along that side.
`sideOffset` | `number \| undefined` | 8 | Gap between the trigger and the panel, in pixels.
`alignOffset` | `number \| undefined` | 0 | Shifts the panel along the alignment axis, in pixels.
`closeOnEsc` | `boolean \| undefined` | true | Escape key closes the panel.
`closeOnOutside` | `boolean \| undefined` | true | Clicking outside the panel closes it.
`beforeClose` | `((done: () => void) => void) \| undefined` |  | Custom exit animation; call `done()` when it's complete. Delays the actual close/unmount until then.
`forceMount` | `boolean \| undefined` | false | When true, presence is v-show-driven and owned by the consumer (e.g. AnimatePresence).
`teleportTo` | `string \| HTMLElement \| undefined` |  | CSS selector or an actual DOM element — same contract as Vue's own Teleport `to`. Wins over `container` either way.
`container` | `DOMTarget \| undefined` |  | Scopes the popover to one element: it teleports there instead of `body`, positions against it, and Escape-key ownership is scoped to it too, so it doesn't contend with page-level layers. Omit for a page-level popover.
`scrollFade` | `boolean \| undefined` | true | Masks the panel's top/bottom edge as its content scrolls under it, signaling there's more.
`ui` | `Partial<{ positioner: UiPartValue; panel: UiPartValue; }> \| undefined` |  | Per-instance part-class/style overrides.
`open` | `boolean \| undefined` | false | Whether the popover is open.

## Slots

Name | Type | Description
--- | --- | ---
`default` | `{ close: () => void; open: boolean; isClosing: boolean; cancelClose: () => void; panelEl: HTMLElement \| null; placement: string; }` | 
`trigger` | `{ open: boolean; setTriggerEl: (el: any) => void; }` | Co-located trigger markup — bind `:ref="setTriggerEl"` on whatever you render here.

## Events

Name | Type | Description
--- | --- | ---
`open-change` | `[value: boolean, details: PopoverOpenChangeDetails]` | 
`update:open` | `[value: boolean]` | 

## Exposed

Name | Type | Description
--- | --- | ---
`panelEl` | `HTMLElement \| null` | 
`positionerEl` | `HTMLElement \| null` | 
`placement` | `Placement` | 
`positionerStyle` | `Record<string, string>` | 
`isClosing` | `boolean` | 
`close` | `() => void` | 
`cancelClose` | `() => void` | 


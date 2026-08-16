# Tooltip

A small floating label that appears on hover or focus to explain an element.

```ts
import { Tooltip } from 'vael-ui' // or 'vael-ui/vapor'
```

## Props

Name | Type | Default | Description
--- | --- | --- | ---
`triggerEl` | `TriggerRef` |  | External trigger ref (raw element or `{ el }`-exposing component).
`side` | `Side \| undefined` | "top" | Which side of the trigger the tooltip opens on.
`align` | `Align \| undefined` | "center" | How the tooltip aligns against the trigger along that side.
`sideOffset` | `number \| undefined` | 8 | Gap between the trigger and the tooltip, in pixels.
`alignOffset` | `number \| undefined` | 0 | Shifts the tooltip along the alignment axis, in pixels.
`openDelay` | `number \| undefined` | 400 | Delay before a cold open, ms. Warm-group opens (another tooltip visible or just hidden) are always instant.
`closeDelay` | `number \| undefined` | 100 | Grace period after the pointer leaves, ms — long enough to travel onto the tooltip.
`interactive` | `boolean \| undefined` | true | Hovering the tooltip itself keeps it open (selectable/clickable content).
`closeOnEsc` | `boolean \| undefined` | true | Escape key closes the tooltip.
`beforeClose` | `((done: () => void) => void) \| undefined` |  | Custom exit animation; call `done()` when it's complete. Delays the actual close/unmount until then.
`forceMount` | `boolean \| undefined` | false | When true, presence is v-show-driven and owned by the consumer.
`teleportTo` | `string \| HTMLElement \| undefined` | "body" | CSS selector or DOM element; same as Vue's Teleport `to`.
`ui` | `Partial<{ positioner: UiPartValue; panel: UiPartValue; }> \| undefined` |  | Per-instance part-class/style overrides.
`open` | `boolean \| undefined` | false | Whether the tooltip is open.

## Slots

Name | Type | Description
--- | --- | ---
`default` | `{ open: boolean; isClosing: boolean; }` | 
`trigger` | `{ open: boolean; setTriggerEl: (el: any) => void; }` | Trigger markup; bind `:ref="setTriggerEl"` to the triggering element.

## Events

Name | Type | Description
--- | --- | ---
`open-change` | `[value: boolean, details: TooltipOpenChangeDetails]` | 
`update:open` | `[value: boolean]` | 

## Exposed

Name | Type | Description
--- | --- | ---
`panelEl` | `HTMLElement \| null` | 
`positionerEl` | `HTMLElement \| null` | 
`placement` | `Placement` | 
`positionerStyle` | `Record<string, string>` | 
`isClosing` | `boolean` | 
`show` | `() => void` | 
`hide` | `() => void` | 
`cancelClose` | `() => void` | 


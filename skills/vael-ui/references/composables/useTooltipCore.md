# useTooltipCore

The headless engine behind `<Tooltip>`.

`useTooltip` without its trigger hover, focus and press listeners, so one tooltip can serve many triggers through event delegation. `<TooltipHost>` and `v-tooltip` run on it. It takes the same options and returns the same values as `useTooltip`, plus the handlers below. You route events to them yourself and swap `triggerEl` as the pointer moves between targets. It still binds touch long-press, Escape and positioner hover.

```ts
import { useTooltipCore } from 'vael-ui' // or 'vael-ui/vapor'
```

## Parameters

Name | Type | Description
--- | --- | ---
`open / options` | `Ref<boolean>, UseTooltipOptions` | The same as `useTooltip`. `triggerEl` can change while open: call `beginTravel()` first, and the tooltip glides to the new trigger.

## Returns

Name | Type | Description
--- | --- | ---
`pointerEnter / pointerLeave` | `(event?: PointerEvent) => void` | Call on the current trigger's pointer enter and leave. Enter opens after `openDelay`, or at once when warm, and ignores touch. Leave closes after `closeDelay`.
`focusEnter / focusLeave` | `(event?: Event) => void` | Call on focus in and out. Focus opens only when the trigger matches `:focus-visible`, so a mouse click doesn't open it. Losing focus closes it.
`press` | `(event?: Event) => void` | Call on pointer down. It closes the tooltip and keeps it closed until the pointer leaves.
`beginTravel` | `() => void` | Records where the visible tooltip is, before you swap `triggerEl`, so the next position glides from there. Glides longer than 320 px, or under reduced motion, jump instead.

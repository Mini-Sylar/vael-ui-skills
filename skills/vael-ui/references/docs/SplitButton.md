# SplitButton

A primary action fused with a chevron that opens alternate actions in a dropdown.

```ts
import { SplitButton } from 'vael-ui' // or 'vael-ui/vapor'
```

## Props

Name | Type | Default | Description
--- | --- | --- | ---
`items` | `readonly MenuEntry<T>[]` |  | Dropdown rows — same shape as Menu.vue's `items`.
`variant` | `ButtonVariant \| undefined` | "primary" | 
`size` | `ButtonSize \| undefined` | "md" | 
`disabled` | `boolean \| undefined` | false | 
`loading` | `boolean \| "auto" \| undefined` | "auto" | Forwarded to the main action Button only.
`triggerLabel` | `string \| undefined` |  | aria-label for the chevron button. Default: localized "More actions".
`side` | `Side \| undefined` |  | 
`align` | `Align \| undefined` |  | 
`sideOffset` | `number \| undefined` |  | 
`alignOffset` | `number \| undefined` |  | 
`closeOnEsc` | `boolean \| undefined` | undefined | 
`closeOnOutside` | `boolean \| undefined` | undefined | 
`beforeClose` | `((done: () => void) => void) \| undefined` |  | 
`forceMount` | `boolean \| undefined` |  | 
`teleportTo` | `string \| HTMLElement \| undefined` |  | 
`scrollFade` | `boolean \| undefined` | undefined | 
`ui` | `Partial<{ root: UiPartValue; main: UiPartValue; trigger: UiPartValue; positioner: UiPartValue; panel: UiPartValue; }> \| undefined` |  | 
`open` | `boolean \| undefined` | false | 

## Slots

Name | Type | Description
--- | --- | ---
`default` | `any` | Main action button content.
`item` | `{ item: T; }` | Override one dropdown row's content while keeping its behavior — forwarded to Menu's own `#item`.

## Events

Name | Type | Description
--- | --- | ---
`open-change` | `[value: boolean, details: PopoverOpenChangeDetails]` | 
`select` | `[item: T]` | 
`update:open` | `[value: boolean]` | 

## Exposed

_None._


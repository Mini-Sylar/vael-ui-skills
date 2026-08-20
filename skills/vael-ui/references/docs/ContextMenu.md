# ContextMenu

A menu that opens at the pointer on right-click, replacing the native browser menu.

```ts
import { ContextMenu } from 'vael-ui' // or 'vael-ui/vapor'
```

## Props

Name | Type | Default | Description
--- | --- | --- | ---
`items` | `readonly MenuEntry<T>[] \| undefined` |  | Data-driven rows — same shape as Menu.vue's `items`.
`disabled` | `boolean \| undefined` | false | Suppresses both the right-click and long-press triggers entirely.
`longPress` | `boolean \| undefined` | true | Long-press (touch) as an additional trigger alongside the native `contextmenu` event.
`longPressDelay` | `number \| undefined` | 500 | Hold duration, in ms, before a touch press opens the menu.
`side` | `Side \| undefined` | "bottom" | Which corner of the cursor point the panel expands from. Default: top-left corner.
`align` | `Align \| undefined` | "start" | How the panel aligns against the cursor point along that side.
`sideOffset` | `number \| undefined` | 2 | Gap between the cursor point and the panel, in pixels.
`alignOffset` | `number \| undefined` | 0 | Shifts the panel along the alignment axis, in pixels.
`closeOnEsc` | `boolean \| undefined` | true | Escape key closes the menu.
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
`default` | `{ open: boolean; }` | Arbitrary wrapped content — a card, a table row, an image. Right-click (or long-press on touch) opens the menu; ordinary interaction with it is untouched.
`item` | `{ item: T; }` | Override one data-driven row's content while keeping its behavior.

## Events

Name | Type | Description
--- | --- | ---
`open-change` | `[value: boolean, details: PopoverOpenChangeDetails]` | 
`select` | `[item: T]` | 
`update:open` | `[value: boolean]` | 

## Exposed

Name | Type | Description
--- | --- | ---
`wrapperEl` | `unknown` | Type inference unavailable — vue-component-meta cannot resolve defineExpose on this generic component.
`anchorEl` | `unknown` | Type inference unavailable — vue-component-meta cannot resolve defineExpose on this generic component.
`panelEl` | `unknown` | Type inference unavailable — vue-component-meta cannot resolve defineExpose on this generic component.
`positionerEl` | `unknown` | Type inference unavailable — vue-component-meta cannot resolve defineExpose on this generic component.
`listEl` | `unknown` | Type inference unavailable — vue-component-meta cannot resolve defineExpose on this generic component.
`isClosing` | `unknown` | Type inference unavailable — vue-component-meta cannot resolve defineExpose on this generic component.
`openAt` | `unknown` | Type inference unavailable — vue-component-meta cannot resolve defineExpose on this generic component.
`close` | `unknown` | Type inference unavailable — vue-component-meta cannot resolve defineExpose on this generic component.
`cancelClose` | `unknown` | Type inference unavailable — vue-component-meta cannot resolve defineExpose on this generic component.


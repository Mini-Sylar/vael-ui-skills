# CommandPalette

```ts
import { CommandPalette } from 'vael-ui' // or 'vael-ui/vapor'
```

## Props

Name | Type | Default | Description
--- | --- | --- | ---
`items` | `readonly T[]` |  | 
`placeholder` | `string \| undefined` |  | 
`filter` | `((item: T, query: string) => boolean) \| undefined` |  | 
`shortcut` | `string \| undefined` |  | Global shortcut that toggles `open`, e.g. `'mod+k'` (`mod` = Cmd on Mac, Ctrl elsewhere). Unset by default — nothing listens until you opt in.
`closeOnSelect` | `boolean \| undefined` | true | 
`size` | `DialogSize \| undefined` | "lg" | 
`position` | `DialogPosition \| undefined` | "top" | Where the panel anchors in the viewport. `'top'` (default) matches the Spotlight/Raycast convention; `'center'` for a more Dialog-like feel.
`modal` | `boolean \| undefined` | true | 
`closeOnEsc` | `boolean \| undefined` | true | 
`closeOnOverlay` | `boolean \| undefined` | true | 
`beforeClose` | `((done: () => void) => void) \| undefined` |  | Custom exit animation; call `done()` when complete. Forwarded straight to the underlying `Dialog`.
`forceMount` | `boolean \| undefined` | false | Presence becomes `v-show`-driven, owned by the consumer. Forwarded straight to the underlying `Dialog`.
`teleportTo` | `string \| undefined` |  | 
`container` | `DOMTarget \| undefined` |  | Scopes the palette to one element instead of the viewport. Forwarded straight to the underlying `Dialog`.
`scrollTarget` | `DOMTarget \| undefined` |  | Element whose scrolling is locked while open, if different from `container`. Forwarded straight to the underlying `Dialog`.
`ui` | `Partial<{ panel: UiPartValue; input: UiPartValue; list: UiPartValue; groupLabel: UiPartValue; item: UiPartValue; empty: UiPartValue; }> \| undefined` |  | 
`open` | `boolean \| undefined` | false | 
`query` | `string \| undefined` | "" | 

## Slots

Name | Type | Description
--- | --- | ---
`item` | `{ item: T; index: number; active: boolean; select: () => void; }` | 
`empty` | `any` | 

## Events

Name | Type | Description
--- | --- | ---
`open-change` | `[value: boolean, details: DialogOpenChangeDetails]` | 
`select` | `[item: T]` | 
`update:open` | `[value: boolean]` | 
`update:query` | `[value: string]` | 

## Exposed

Name | Type | Description
--- | --- | ---
`panelEl` | `unknown` | Type inference unavailable — vue-component-meta cannot resolve defineExpose on this generic component.
`isClosing` | `unknown` | Type inference unavailable — vue-component-meta cannot resolve defineExpose on this generic component.
`close` | `unknown` | Type inference unavailable — vue-component-meta cannot resolve defineExpose on this generic component.
`cancelClose` | `unknown` | Type inference unavailable — vue-component-meta cannot resolve defineExpose on this generic component.
`inputEl` | `unknown` | Type inference unavailable — vue-component-meta cannot resolve defineExpose on this generic component.
`listEl` | `unknown` | Type inference unavailable — vue-component-meta cannot resolve defineExpose on this generic component.
`filteredItems` | `unknown` | Type inference unavailable — vue-component-meta cannot resolve defineExpose on this generic component.
`activeIndex` | `unknown` | Type inference unavailable — vue-component-meta cannot resolve defineExpose on this generic component.


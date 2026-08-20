# Drawer

A panel that slides in from a viewport edge, for navigation or side content.

```ts
import { Drawer } from 'vael-ui' // or 'vael-ui/vapor'
```

## Props

Name | Type | Default | Description
--- | --- | --- | ---
`side` | `DrawerSide \| undefined` | "right" | Which viewport edge the panel slides in from.
`size` | `DialogSize \| undefined` | "md" | Panel width: sm 22rem / md 28rem / lg 38rem.
`ui` | `Partial<{ overlay: UiPartValue; panel: UiPartValue; header: UiPartValue; title: UiPartValue; description: UiPartValue; body: UiPartValue; footer: UiPartValue; }> \| undefined` |  | Per-instance part-class/style overrides.
`closeOnEsc` | `boolean \| undefined` | true | Escape key closes the panel.
`beforeClose` | `((done: () => void) => void) \| undefined` |  | Custom exit animation; call `done()` when it's complete. Delays the actual close/unmount until then.
`forceMount` | `boolean \| undefined` | false | When true, presence is v-show-driven and owned by the consumer (e.g. AnimatePresence).
`teleportTo` | `string \| undefined` |  | Teleport target for the panel/overlay.
`scrollFade` | `boolean \| undefined` | true | Masks the panel's top/bottom edge as its content scrolls under it, signaling there's more.
`title` | `string \| undefined` |  | Renders the default header and wires aria-labelledby automatically.
`description` | `string \| undefined` |  | Muted line under the title; wires aria-describedby automatically.
`role` | `"dialog" \| "alertdialog" \| undefined` | "dialog" | `alertdialog` for urgent messages requiring a response (e.g. confirmations) — announced more assertively by screen readers.
`container` | `DOMTarget \| undefined` |  | Scopes the dialog to one element instead of the viewport: the overlay dims only that box, scroll lock and modality apply only inside it, and the rest of the page stays interactive. Also becomes the teleport target unless `teleportTo` is set. Given a positioning context automatically if it doesn't already have one.
`scrollTarget` | `DOMTarget \| undefined` |  | Element whose scrolling is locked while open. Defaults to `container`. Pass the inner scroller when the container itself doesn't scroll - an absolutely-positioned panel scrolls away with its container's content.
`initialFocus` | `(() => HTMLElement \| null \| undefined) \| undefined` |  | Custom initial focus; return null/undefined to use default (first focusable).
`showClose` | `boolean \| undefined` | true | Hide the built-in × when the footer carries the only sensible actions.
`modal` | `boolean \| undefined` | true | `false` disables overlay, scroll lock, and focus trap. Escape-close and layer stacking still apply.
`flush` | `boolean \| undefined` |  | `true` removes edge padding; `top`/`bottom` panels sit flush to the viewport edge instead of floating.
`closeOnOverlay` | `boolean \| undefined` | true | Clicking the overlay closes the panel. No-op when `modal` is false (no overlay to click).
`maximizable` | `boolean \| undefined` |  | Adds a maximize/restore toggle to the header, filling the viewport when active.
`open` | `boolean \| undefined` | false | Whether the drawer is open.
`maximized` | `boolean \| undefined` | false | Whether the panel currently fills the viewport. Self-managed by Dialog's built-in toggle unless the consumer binds it.

## Slots

Name | Type | Description
--- | --- | ---
`default` | `{ close: () => void; open: boolean; isClosing: boolean; cancelClose: () => void; panelEl: HTMLElement \| null; }` | 
`header` | `{ close: () => void; }` | 
`footer` | `{ close: () => void; }` | 

## Events

Name | Type | Description
--- | --- | ---
`open-change` | `[value: boolean, details: DialogOpenChangeDetails]` | 
`update:open` | `[value: boolean]` | 
`update:maximized` | `[value: boolean]` | 

## Exposed

Name | Type | Description
--- | --- | ---
`panelEl` | `HTMLElement \| null` | 
`isClosing` | `boolean` | 
`close` | `() => void \| undefined` | 
`cancelClose` | `() => void \| undefined` | 


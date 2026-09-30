# Dialog

A modal overlay that interrupts the page until the user responds or dismisses it.

```ts
import { Dialog } from 'vael-ui' // or 'vael-ui/vapor'
```

## Props

Name | Type | Default | Description
--- | --- | --- | ---
`title` | `string \| undefined` |  | Renders the default header and wires `aria-labelledby` automatically.
`description` | `string \| undefined` |  | Muted line under the title; wires `aria-describedby` automatically.
`size` | `DialogSize \| undefined` | `'md'` | Panel width: `'sm'` is 22rem, `'md'` is 28rem, `'lg'` is 38rem.
`position` | `DialogPosition \| undefined` | `'center'` | Where the panel anchors in the viewport.
`role` | `"dialog" \| "alertdialog" \| undefined` | `'dialog'` | Use `'alertdialog'` for urgent messages that need a response, such as confirmations. Screen readers announce it more assertively.
`initialFocus` | `(() => HTMLElement \| null \| undefined) \| undefined` |  | Returns the element to focus on open; return `null` or `undefined` to focus the first focusable element.
`showClose` | `boolean \| undefined` | `true` | Shows the built-in close button (×). Hide it when the footer carries the only sensible actions.
`modal` | `boolean \| undefined` | `true` | `false` disables the overlay, scroll lock and focus trap. Escape-close and layer stacking still apply.
`flush` | `boolean \| undefined` | `false` | `true` removes edge padding; `top`/`bottom` panels sit flush to the viewport edge instead of floating.
`closeOnEsc` | `boolean \| undefined` | `true` | Escape key closes the panel.
`closeOnOverlay` | `boolean \| undefined` | `true` | Clicking the overlay closes the panel. Has no effect when `modal` is `false` (no overlay to click).
`closeOnHistoryBack` | `boolean \| undefined` | `false` | The browser or mobile back action closes the panel instead of leaving the page. Opening pushes a history entry, and any other close removes it.
`beforeClose` | `((done: () => void) => void) \| undefined` |  | Custom exit animation; call `done()` to finish closing.
`forceMount` | `boolean \| undefined` | `false` | Keeps it mounted, toggled with `v-show`, so you can own the enter/exit animation.
`teleportTo` | `string \| undefined` |  | Teleport target: a CSS selector. Wins over `container` either way.
`container` | `DOMTarget \| undefined` |  | Scopes the panel to an element instead of the viewport. The overlay, scroll lock and modality apply only inside it. It's also the teleport target unless you set `teleportTo`.
`scrollTarget` | `DOMTarget \| undefined` |  | Element to scroll-lock while open. Defaults to `container`; pass the inner scroller when the container itself doesn't scroll.
`scrollFade` | `boolean \| undefined` | `true` | Masks the panel's top/bottom edge as its content scrolls under it, signaling there's more.
`maximizable` | `boolean \| undefined` | `false` | Adds a maximize/restore toggle to the header, filling the viewport when active.
`ui` | `Partial<{ overlay: UiPartValue; panel: UiPartValue; header: UiPartValue; title: UiPartValue; description: UiPartValue; body: UiPartValue; footer: UiPartValue; maximize: UiPartValue; close: UiPartValue; }> \| undefined` |  | Class and style overrides for each part.
`open` | `boolean \| undefined` | `false` | Whether the dialog is open.
`maximized` | `boolean \| undefined` | `false` | Whether the panel fills the viewport. The built-in toggle manages it unless you bind `v-model:maximized`.

## Slots

Name | Type | Description
--- | --- | ---
`default` | `{ close: () => void; open: boolean; isClosing: boolean; cancelClose: () => void; panelEl: HTMLElement \| null; }` | Body content; receives `close`, `open`, `isClosing`, `cancelClose` and `panelEl`.
`header` | `{ close: () => void; }` | Replaces the default title/description header.
`footer` | `{ close: () => void; }` | Action row at the end of the panel.

## Events

Name | Type | Description
--- | --- | ---
`open-change` | `[value: boolean, details: DialogOpenChangeDetails]` | Fires when a close is requested; call `details.cancel()` to keep the dialog open.
`update:open` | `[value: boolean]` | Fires when `open` changes (`v-model:open`).
`update:maximized` | `[value: boolean]` | Fires when `maximized` changes (`v-model:maximized`).

## Exposed

Name | Type | Description
--- | --- | ---
`panelEl` | `HTMLElement \| null` | Panel element (`null` while closed).
`isClosing` | `boolean` | `true` while a `beforeClose` close is pending.
`close` | `() => void` | Closes the dialog, running `@open-change` and `beforeClose` first.
`cancelClose` | `() => void` | Cancels a close pending in `beforeClose` and keeps the dialog open.
`maximized` | `boolean` | Whether the panel is maximized. 


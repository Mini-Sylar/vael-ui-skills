# Drawer

A panel that slides in from a viewport edge, for navigation or side content.

```ts
import { Drawer } from 'vael-ui' // or 'vael-ui/vapor'
```

## Props

Name | Type | Default | Description
--- | --- | --- | ---
`side` | `'top' \| 'right' \| 'bottom' \| 'left' \| undefined` | `'right'` | Which viewport edge the panel slides in from.
`size` | `'sm' \| 'md' \| 'lg' \| undefined` | `'md'` | Panel width: `'sm'` is 22rem, `'md'` is 28rem, `'lg'` is 38rem.
`ui` | `Partial<{ overlay: UiPartValue; panel: UiPartValue; header: UiPartValue; title: UiPartValue; description: UiPartValue; body: UiPartValue; footer: UiPartValue; maximize: UiPartValue; close: UiPartValue; }> \| undefined` |  | Class and style overrides for each part.
`motion` | `DialogMotion \| undefined` |  | How it moves: `false` for instantly, `{ spring }` to retime the built-in motion, states by part (`initial: { panel: { … } }`), your `targets`, or `enter`/`leave` hooks for a library. Unset, it follows the nearest ConfigProvider's style.
`closeOnEsc` | `boolean \| undefined` | `true` | Escape key closes the panel.
`beforeClose` | `BeforeClose<DialogCloseReason> \| undefined` |  | Runs before the dialog closes. Return `true` to close it, `false` to keep it open, or a promise of either, like `() => confirmAction(…).result`. The dialog stays open while the promise is pending.
`forceMount` | `boolean \| undefined` | `false` | Keeps it mounted while closed, toggled with `v-show`, so its content keeps its state. vael-ui then leaves its motion to you.
`teleportTo` | `string \| undefined` |  | Teleport target: a CSS selector. Wins over `container` either way.
`scrollFade` | `boolean \| undefined` | `true` | Masks the panel's top/bottom edge as its content scrolls under it, signaling there's more.
`container` | `DOMTarget \| undefined` |  | Scopes the panel to an element instead of the viewport. The overlay, scroll lock and modality apply only inside it. It's also the teleport target unless you set `teleportTo`.
`title` | `string \| undefined` |  | Renders the default header and wires `aria-labelledby` automatically.
`description` | `string \| undefined` |  | Muted line under the title; wires `aria-describedby` automatically.
`role` | `"dialog" \| "alertdialog" \| undefined` | `'dialog'` | Use `'alertdialog'` for urgent messages that need a response, such as confirmations. Screen readers announce it more assertively.
`origin` | `"trigger" \| "center" \| undefined` | `'center'` | `'trigger'` zooms the panel out of the element that opened it, and back into it on close, for a dialog that expands from a card or a button. Centred dialogs only.
`initialFocus` | `(() => HTMLElement \| null \| undefined) \| undefined` |  | Returns the element to focus on open; return `null` or `undefined` to focus the first focusable element.
`showClose` | `boolean \| undefined` | `true` | Shows the built-in close button (×). Hide it when the footer carries the only sensible actions.
`modal` | `boolean \| undefined` | `true` | `false` turns off the overlay, scroll lock, focus trap and the inert page behind it. Escape-close and layer stacking still apply.
`flush` | `boolean \| undefined` | `false` | `true` removes edge padding: a `top`/`bottom` Dialog sits flush to the viewport edge instead of floating (a Drawer's always does).
`closeOnOverlay` | `boolean \| undefined` | `true` | Clicking the overlay closes the panel. Has no effect when `modal` is `false` (no overlay to click).
`closeOnHistoryBack` | `boolean \| undefined` | `false` | The browser or mobile back action closes the panel instead of leaving the page. Opening pushes a history entry, and any other close removes it.
`scrollTarget` | `DOMTarget \| undefined` |  | Element to scroll-lock while open. Defaults to `container`; pass the inner scroller when the container itself doesn't scroll.
`maximizable` | `boolean \| undefined` | `false` | Adds a maximize/restore toggle to the header, filling the viewport when active.
`open` | `boolean \| undefined` | `false` | Whether the drawer is open.
`maximized` | `boolean \| undefined` | `false` | Whether the panel fills the viewport. Dialog's built-in toggle manages it unless you bind it.

## Slots

Name | Type | Description
--- | --- | ---
`default` | `{ close: () => void; open: boolean; isClosing: boolean; cancelClose: () => void; panelEl: HTMLElement \| null; }` | Panel content.
`header` | `{ close: () => void; }` | Replaces the default title and description header.
`footer` | `{ close: () => void; }` | Action row at the end of the panel.

## Events

Name | Type | Description
--- | --- | ---
`open-change` | `[value: boolean, details: DialogOpenChangeDetails]` | Fires before the drawer closes, with the reason. Call `details.cancel()` to keep it open.
`update:open` | `[value: boolean]` | Fires when `open` changes (`v-model:open`).
`update:maximized` | `[value: boolean]` | Fires when `maximized` changes (`v-model:maximized`).

## Exposed

Name | Type | Description
--- | --- | ---
`panelEl` | `HTMLElement \| null` | Panel element (null while closed).
`overlayEl` | `HTMLElement \| null` | Backdrop element. `null` while closed or when `modal` is `false`.
`isClosing` | `boolean` | `true` while a `beforeClose` close is pending.
`close` | `() => void \| undefined` | Closes the drawer, running `beforeClose` first.
`cancelClose` | `() => void \| undefined` | Cancels a close pending in `beforeClose` and keeps the drawer open.

## Motion

`motion` takes `DialogMotion` (exported from `'vael-ui'`); `false` turns it off. Dialog's `motion`, and Drawer's and BottomSheet's: values or hooks for its parts (DialogPart), how it opens and closes (`morph`, `linked`, `exitOrder`, `resize`) and swapped content.

- Parts: `overlay`, `panel`, `header`, `title`, `description`, `body`, `footer`, `maximize`, `close`
- Keys: `spring`, `duration`, `easing`, `initial`, `exit`, `enter`, `leave`, `targets`, `morph`, `exitOrder`, `resize`, `onAfterEnter`, `onAfterLeave`, `linked`, `swap`, `direction`, `onAfterSwap`

See the [Motion guide](../guides/motion.md) for what each key does.


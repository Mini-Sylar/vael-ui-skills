# confirmAction

Opens a confirm flow with one function call. It shows a centered dialog by default (`surface: 'dialog'`), or a popover anchored to `triggerEl` with `surface: 'popover'`. TypeScript offers only the options that fit the `surface` you pick. The confirm button shows its loading state while `onConfirm` runs, and the surface closes only when it succeeds. If `onConfirm` rejects, the surface stays open and `onError` fires. `confirmAction` wraps `openDialog` and `openPopover`; use those for anything it doesn't cover.

```ts
import { confirmAction } from 'vael-ui' // or 'vael-ui/vapor'
```

## Parameters

Name | Type | Description
--- | --- | ---
`title` | `string` | The heading. Required on both surfaces.
`description` | `string` | 
`confirmLabel / cancelLabel` | `string` | The button labels. Default `'Confirm'` and `'Cancel'`.
`variant` | `ButtonVariant` | The confirm button's style. Default `'primary'`. Use `'danger'` for destructive actions.
`onConfirm` | `() => unknown \| Promise<unknown>` | Runs on confirm. The surface waits for it and closes when it succeeds.
`onCancel` | `() => void` | 
`onError` | `(error: unknown) => void` | Fires when `onConfirm` rejects.
`confirmButtonProps / cancelButtonProps` | `Partial<ButtonProps>` | Sets any Button prop, beyond the label and variant shortcuts.
`body / bodyProps` | `Component / Record<string, unknown>` | Adds content between the description and the buttons, such as a "type DELETE" input.
`surface` | `'dialog' \| 'popover'` | Where the confirm opens: centered, or anchored to `triggerEl`. Default `'dialog'`.
`position / size` | `DialogPosition / DialogSize` | Dialog's own `position` and `size` props. Only with `surface: 'dialog'`.
`triggerEl` | `TriggerRef` | The element the popover anchors to, as in `openPopover`. Required with `surface: 'popover'`.
`side / align / sideOffset / …` | `PopoverProps` | Passes any other Popover prop through. Only with `surface: 'popover'`.

## Returns

Name | Type | Description
--- | --- | ---
`result` | `Promise<boolean \| undefined>` | Settles with the outcome: `true` on confirm, `false` on cancel, `undefined` when dismissed with Escape or an outside click.
`close` | `(result?: boolean) => void` | Closes the surface from the opener's side.
`panelEl` | `{ readonly value: HTMLElement \| null }` | The surface panel, `null` until it mounts. Use it to animate the panel with GSAP or motion.


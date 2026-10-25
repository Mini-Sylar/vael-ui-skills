# confirmAction

Opens a confirm flow with one function call. It's centered by default (`surface: 'dialog'`), or anchored to a trigger with `surface: 'popover'`, which requires `triggerEl`. TypeScript offers the options that match the `surface` you pick. `onConfirm` is awaited: the confirm button shows its loading state until it settles, and the surface closes only on success. If `onConfirm` rejects, the surface stays open and `onError` fires. `confirmAction` isn't a new component. It wraps `openDialog` and `openPopover`, which you can still use for anything it doesn't cover.

```ts
import { confirmAction } from 'vael-ui' // or 'vael-ui/vapor'
```

## Parameters

Name | Type | Description
--- | --- | ---
`title` | `string` | Required for both surfaces.
`description` | `string` | 
`confirmLabel / cancelLabel` | `string` | Default `'Confirm'` / `'Cancel'`.
`variant` | `ButtonVariant` | Confirm button style. Default `'primary'`. Use `'danger'` for destructive actions.
`onConfirm` | `() => unknown \| Promise<unknown>` | Awaited before the surface closes.
`onCancel` | `() => void` | 
`onError` | `(error: unknown) => void` | Fires when `onConfirm` rejects.
`confirmButtonProps / cancelButtonProps` | `Partial<ButtonProps>` | Passes any Button prop, beyond the label and variant shortcuts.
`body / bodyProps` | `Component / Record<string, unknown>` | Extra content between the description and the buttons, such as a "type DELETE" input.
`surface` | `'dialog' \| 'popover'` | Default `'dialog'`.
`position / size` | `DialogPosition / DialogSize` | Only with `surface: 'dialog'`. Same as Dialog's own props.
`triggerEl` | `TriggerRef` | Only with `surface: 'popover'`, where it's required. Same contract as `openPopover`'s `triggerEl`.
`side / align / sideOffset / …` | `PopoverProps` | Only with `surface: 'popover'`. Any other Popover prop, passed through.

## Returns

Name | Type | Description
--- | --- | ---
`result` | `Promise<boolean \| undefined>` | `true` on confirm, `false` on cancel, `undefined` when dismissed with Escape or an outside click.
`close` | `(result?: boolean) => void` | Closes the surface from the opener's side.
`panelEl` | `{ readonly value: HTMLElement \| null }` | `null` until the surface mounts. Use it for GSAP or motion-v enter animations.

# confirmAction

One function for async-aware confirm flows, either centered (`surface: 'dialog'`, the default) or anchored to a trigger (`surface: 'popover'`, requires `triggerEl`); a discriminated union on `surface` picks which options TypeScript actually offers you. `onConfirm` is awaited before closing: the confirm button stays in its loading state until it settles, and the surface closes only on success. A rejection leaves it open and fires `onError` instead of closing out from under a failed action. Not a new component, just sugar over `openDialog`/`openPopover`, both still there for anything this doesn’t cover.

## Parameters

Name | Type | Description
--- | --- | ---
`title` | `string` | Required either way.
`description` | `string` | 
`confirmLabel / cancelLabel` | `string` | Default 'Confirm' / 'Cancel'.
`variant` | `ButtonVariant` | Confirm button style. Default 'primary'; use 'danger' for destructive actions.
`onConfirm` | `() => unknown \| Promise<unknown>` | Awaited before closing.
`onCancel` | `() => void` | 
`onError` | `(error: unknown) => void` | Fires when onConfirm rejects.
`confirmButtonProps / cancelButtonProps` | `Partial<ButtonProps>` | Full prop passthrough beyond the label/variant shortcuts.
`body / bodyProps` | `Component / Record<string, unknown>` | Extra content between the description and the buttons, e.g. a "type DELETE" input.
`surface` | `'dialog' \| 'popover'` | Default 'dialog'.
`position / size` | `DialogPosition / DialogSize` | surface: 'dialog' only, same as Dialog's own props.
`triggerEl` | `TriggerRef` | surface: 'popover' only. Required, same contract as openPopover's own triggerEl.
`side / align / sideOffset / …` | `PopoverProps` | surface: 'popover' only. Every other Popover prop, passed straight through.

## Returns

Name | Type | Description
--- | --- | ---
`result` | `Promise<boolean \| undefined>` | true on confirm, false on cancel, undefined on Escape/outside-click dismissal.
`close` | `(result?: boolean) => void` | Close imperatively from the opener's side.
`panelEl` | `{ readonly value: HTMLElement \| null }` | Null until the surface actually mounts; useful for GSAP/motion-v enter animations.


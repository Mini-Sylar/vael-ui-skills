# Message

An inline banner for status text: info, success, warning, or error.

```ts
import { Message } from 'vael-ui' // or 'vael-ui/vapor'
```

## Props

Name | Type | Default | Description
--- | --- | --- | ---
`title` | `string \| undefined` |  | Bold heading above the default slot content.
`variant` | `MessageVariant \| undefined` | `'default'` | Color variant; sets the icon and border.
`appearance` | `"default" \| "bare" \| undefined` | `'default'` | `'bare'` drops the border, background and padding, leaving only the icon and colored text. Use it inline, such as for form field validation, instead of as a standalone banner.
`closable` | `boolean \| undefined` | `false` | Renders the built-in dismiss button.
`beforeClose` | `((done: () => void) => void) \| undefined` |  | Custom exit animation; call `done()` to finish closing. The model stays `true` until you do.
`forceMount` | `boolean \| undefined` | `false` | Keeps it mounted, toggled with `v-show`, so you can own the enter/exit animation.
`showIcon` | `boolean \| undefined` | `true` | Shows the leading status icon (or the `#icon` slot).
`role` | `"status" \| "alert" \| undefined` |  | Defaults to `'alert'` for `'error'` and `'warning'`, `'status'` otherwise.
`ui` | `Partial<{ root: UiPartValue; icon: UiPartValue; content: UiPartValue; title: UiPartValue; description: UiPartValue; actions: UiPartValue; close: UiPartValue; }> \| undefined` |  | Class and style overrides for each part.
`open` | `boolean \| undefined` | `true` | Whether the message is shown.

## Slots

Name | Type | Description
--- | --- | ---
`default` | `any` | Description body, under the optional title.
`icon` | `any` | Replaces the default StatusIcon.
`actions` | `any` | Trailing action row, before the close button.

## Events

Name | Type | Description
--- | --- | ---
`open-change` | `[value: boolean, details: MessageOpenChangeDetails]` | Fires before the model flips to false; `details.cancel()` vetoes the close.
`update:open` | `[value: boolean]` | Fires when `open` changes (`v-model:open`).

## Exposed

Name | Type | Description
--- | --- | ---
`el` | `HTMLElement \| null` | Root element (null while closed unless `forceMount`).
`close` | `() => void` | Closes the message, running `@open-change` and `beforeClose` first.
`isClosing` | `boolean` | `true` while a `beforeClose` close is pending.
`cancelClose` | `() => void` | Cancels a close pending in `beforeClose` and keeps the message open. 


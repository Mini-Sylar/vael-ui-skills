# Message

An inline banner for status text: info, success, warning, or error.

```ts
import { Message } from 'vael-ui' // or 'vael-ui/vapor'
```

## Props

Name | Type | Default | Description
--- | --- | --- | ---
`title` | `string \| undefined` |  | 
`variant` | `MessageVariant \| undefined` | "default" | 
`appearance` | `"default" \| "bare" \| undefined` | "default" | `bare` drops the border/background/padding — icon + colored text only, for inline use (e.g. form field validation) instead of a standalone banner.
`closable` | `boolean \| undefined` | false | Renders the built-in dismiss button.
`beforeClose` | `((done: () => void) => void) \| undefined` |  | Called before the model flips to false. Call `done()` to actually close.
`forceMount` | `boolean \| undefined` | false | When true, presence is v-show-driven and owned by the consumer (e.g. AnimatePresence).
`showIcon` | `boolean \| undefined` | true | 
`role` | `"status" \| "alert" \| undefined` |  | Defaults to `alert` for `error`/`warning`, `status` otherwise.
`ui` | `Partial<{ root: UiPartValue; icon: UiPartValue; content: UiPartValue; title: UiPartValue; description: UiPartValue; close: UiPartValue; }> \| undefined` |  | 
`open` | `boolean \| undefined` | true | 

## Slots

Name | Type | Description
--- | --- | ---
`default` | `any` | Description body, under the optional title.
`icon` | `any` | Replaces the default StatusIcon.
`actions` | `any` | Trailing action row, before the close button.

## Events

Name | Type | Description
--- | --- | ---
`open-change` | `[value: boolean, details: MessageOpenChangeDetails]` | 
`update:open` | `[value: boolean]` | 

## Exposed

Name | Type | Description
--- | --- | ---
`el` | `HTMLElement \| null` | 
`close` | `() => void` | 
`isClosing` | `boolean` | 
`cancelClose` | `() => void` | 


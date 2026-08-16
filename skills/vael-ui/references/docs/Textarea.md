# Textarea

A multi-line text field that can auto-grow with its content.

```ts
import { Textarea } from 'vael-ui' // or 'vael-ui/vapor'
```

## Props

Name | Type | Default | Description
--- | --- | --- | ---
`size` | `"md" \| "sm" \| "lg" \| undefined` | "md" | 
`disabled` | `boolean \| undefined` | false | 
`readonly` | `boolean \| undefined` | false | 
`invalid` | `boolean \| undefined` | false | Standalone override; ORed with the nearest Field's `error` state.
`placeholder` | `string \| undefined` |  | 
`rows` | `number \| undefined` | 3 | Native `rows` attribute — also the auto-grow minimum.
`autoGrow` | `boolean \| undefined` | false | Enable auto-grow behavior.
`maxRows` | `number \| undefined` |  | Only meaningful with `autoGrow`; omitted means no cap.
`ui` | `Partial<{ root: UiPartValue; textarea: UiPartValue; start: UiPartValue; end: UiPartValue; bottomStart: UiPartValue; bottomEnd: UiPartValue; }> \| undefined` |  | 
`modelValue` | `string \| undefined` | "" | 

## Slots

Name | Type | Description
--- | --- | ---
`start` | `any` | Inline leading content (centered on the resting height).
`end` | `any` | 
`bottom-start` | `any` | Bottom-left of the action strip (attachment button, …).
`bottom-end` | `any` | Bottom-right of the action strip (char counter, send button, …).

## Events

Name | Type | Description
--- | --- | ---
`update:modelValue` | `[value: string]` | 

## Exposed

Name | Type | Description
--- | --- | ---
`el` | `HTMLElement \| null` | 
`textareaEl` | `HTMLTextAreaElement \| null` | 


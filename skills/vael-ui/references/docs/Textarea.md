# Textarea

A multi-line text field that can auto-grow with its content.

```ts
import { Textarea } from 'vael-ui' // or 'vael-ui/vapor'
```

## Props

Name | Type | Default | Description
--- | --- | --- | ---
`size` | `"md" \| "sm" \| "lg" \| undefined` | `'md'` | Control size.
`disabled` | `boolean \| undefined` | `false` | Disables the textarea and blocks interaction. A disabled parent Field also disables it.
`readonly` | `boolean \| undefined` | `false` | Makes the value read-only while keeping it focusable and selectable.
`invalid` | `boolean \| undefined` | `false` | Standalone override; ORed with the nearest Field's `error` state.
`placeholder` | `string \| undefined` |  | Text shown while the textarea is empty.
`rows` | `number \| undefined` | `3` | Native `rows` attribute; also sets the auto-grow minimum.
`autoGrow` | `boolean \| undefined` | `false` | Grows the textarea to fit its content.
`maxRows` | `number \| undefined` |  | Maximum rows `autoGrow` grows to; unset means no cap.
`ui` | `Partial<{ root: UiPartValue; textarea: UiPartValue; start: UiPartValue; end: UiPartValue; bottomStart: UiPartValue; bottomEnd: UiPartValue; }> \| undefined` |  | Class and style overrides for each part.
`modelValue` | `string \| undefined` | `''` | Textarea value. Supports the `.trim` and `.lazy` modifiers.

## Slots

Name | Type | Description
--- | --- | ---
`start` | `any` | Inline leading content (centered on the resting height).
`end` | `any` | Inline trailing content (centered on the resting height).
`bottom-start` | `any` | Bottom-left of the action strip, such as an attachment button.
`bottom-end` | `any` | Bottom-right of the action strip, such as a character counter or send button.

## Events

Name | Type | Description
--- | --- | ---
`update:modelValue` | `[value: string]` | Fires when `modelValue` changes (`v-model`).

## Exposed

Name | Type | Description
--- | --- | ---
`el` | `HTMLElement \| null` | Root element (the frame around the textarea).
`textareaEl` | `HTMLTextAreaElement \| null` | Native `<textarea>` element. 


# Switch

An on/off toggle styled like a physical switch, for immediate-effect settings.

```ts
import { Switch } from 'vael-ui' // or 'vael-ui/vapor'
```

## Props

Name | Type | Default | Description
--- | --- | --- | ---
`label` | `string \| undefined` |  | Label text; the default slot replaces it.
`disabled` | `boolean \| undefined` | `false` | Disables the switch and blocks interaction.
`invalid` | `boolean \| undefined` | `false` | Shows the invalid state. A surrounding `Field` in error sets it too.
`size` | `"md" \| "sm" \| undefined` | `'md'` | Control size.
`name` | `string \| undefined` |  | Native `name` for form submission.
`motionCss` | `boolean \| undefined` | `true` | `false` skips built-in transitions; animate the exposed `trackEl`/`thumbEl` yourself.
`ui` | `Partial<{ root: UiPartValue; track: UiPartValue; thumb: UiPartValue; label: UiPartValue; }> \| undefined` |  | Class and style overrides for each part.
`modelValue` | `boolean \| undefined` | `false` | Whether the switch is on.

## Slots

Name | Type | Description
--- | --- | ---
`default` | `any` | Label content; replaces the `label` text.

## Events

Name | Type | Description
--- | --- | ---
`update:modelValue` | `[value: boolean]` | Fires when `modelValue` changes (`v-model`).

## Exposed

Name | Type | Description
--- | --- | ---
`el` | `HTMLElement \| null` | Root element.
`inputEl` | `HTMLInputElement \| null` | Native checkbox input (`role="switch"`).
`trackEl` | `HTMLElement \| null` | Track element.
`thumbEl` | `HTMLElement \| null` | Thumb element. 


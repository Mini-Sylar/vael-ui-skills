# Chip

A removable, compact token for selected filters, tags, or multi-select values.

```ts
import { Chip } from 'vael-ui' // or 'vael-ui/vapor'
```

## Props

Name | Type | Default | Description
--- | --- | --- | ---
`label` | `string \| undefined` |  | Label text. Also names the remove button, so pass it even when using the default slot.
`removable` | `boolean \| undefined` | `false` | Shows a remove button that fires `@remove`.
`disabled` | `boolean \| undefined` | `false` | Disables the remove button.
`size` | `"md" \| "sm" \| undefined` | `'md'` | Chip size.
`ui` | `Partial<{ root: UiPartValue; label: UiPartValue; remove: UiPartValue; }> \| undefined` |  | Class and style overrides for each part.

## Slots

Name | Type | Description
--- | --- | ---
`default` | `any` | Label content; replaces the `label` text. The remove button keeps its name from `label`.

## Events

Name | Type | Description
--- | --- | ---
`remove` | `[]` | Fires when you click the remove button, or press Delete/Backspace on a focused removable chip.

## Exposed

Name | Type | Description
--- | --- | ---
`el` | `HTMLElement \| null` | Root element. 


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
`size` | `"sm" \| "md" \| undefined` | `'md'` | Chip size.
`ui` | `Partial<{ root: UiPartValue; label: UiPartValue; remove: UiPartValue; }> \| undefined` |  | Class and style overrides for each part.
`motion` | `ChipMotion \| undefined` |  | How it moves: its appearance (`{ initial: { root: { opacity: 0, scale: 0.9 } } }`) and its press states (`{ states: { active: { root: { scale: '0.96' } } } }`). `false` turns its own animation off.

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
`parts` | `Record<string, HTMLElement \| HTMLElement[] \| null>` | Its elements by part name, for your own animation; a repeated part is an array.

## Motion

`motion` takes `ChipMotion` (exported from `'vael-ui'`); `false` turns it off. Chip: its appearance and press states.

- Parts: `root`, `remove`
- States: `disabled`, `focus`, `active`, `hover`
- Keys: `spring`, `duration`, `easing`, `initial`, `states`, `replay`, `appear`

See the [Motion guide](../guides/motion.md) for what each key does.


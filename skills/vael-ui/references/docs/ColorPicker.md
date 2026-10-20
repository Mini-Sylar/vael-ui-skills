# ColorPicker
## Props

Name | Type | Default | Description
--- | --- | --- | ---
`presets` | `string[] \| undefined` |  | Colors offered as swatches in the panel, as `#rrggbb`.
`showEyeDropper` | `boolean \| undefined` | `true` | Shows the eyedropper button in browsers that have one.
`label` | `string \| undefined` | `'Color'` | Accessible label for the swatch and the panel.
`name` | `string \| undefined` |  | Native `name`, so a plain `<form>` submits the hex.
`disabled` | `boolean \| undefined` | `false` | Disables the swatch.
`size` | `"sm" \| "md" \| "lg" \| undefined` | `'md'` | Swatch size.
`teleportTo` | `string \| HTMLElement \| undefined` | `'body'` | Where the panel is teleported to.
`motionCss` | `boolean \| undefined` | `true` | `false` skips the built-in transitions; use the exposed elements for your own.
`ui` | `Partial<{ root: UiPartValue; swatch: UiPartValue; panel: UiPartValue; value: UiPartValue; presets: UiPartValue; preset: UiPartValue; }> \| undefined` |  | Class and style overrides for each part.
`modelValue` | `string \| undefined` | `'#000000'` | The color as `#rrggbb`.
`open` | `boolean \| undefined` | `false` | Whether the picker panel is open.

## Slots

Name | Type | Description
--- | --- | ---
`trigger` | `{ color: string; open: boolean; }` | Replaces the swatch; a click on it opens the panel.
`preset` | `{ color: string; selected: boolean; }` | Content inside each preset swatch, such as a check on the selected one.
`default` | `{ color: string; pick: (color: string) => void; }` | Extra panel content under the presets: recent colors, a note, your own controls.

## Events

Name | Type | Description
--- | --- | ---
`update:modelValue` | `[value: string]` | Fires when `modelValue` changes (`v-model`).
`update:open` | `[value: boolean]` | Fires when `open` changes (`v-model:open`).

## Exposed

Name | Type | Description
--- | --- | ---
`el` | `HTMLElement \| null` | Root element.
`swatchEl` | `HTMLButtonElement \| null` | The default swatch button; `null` with a `#trigger` slot.
`panelEl` | `HTMLElement \| null` | The open panel; `null` while closed.
`areaEl` | `HTMLElement \| null` | The saturation and brightness plane; `null` while closed.
`thumbEl` | `HTMLElement \| null` | The plane's thumb; `null` while closed.


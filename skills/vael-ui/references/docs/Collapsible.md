# Collapsible

A single show/hide section for content that is optional or secondary.

```ts
import { Collapsible } from 'vael-ui' // or 'vael-ui/vapor'
```

## Props

Name | Type | Default | Description
--- | --- | --- | ---
`disabled` | `boolean \| undefined` | `false` | Blocks toggling and marks the trigger `aria-disabled`.
`motionCss` | `boolean \| undefined` | `true` | `false` skips transitions; use exposed `panelEl` for custom motion.
`ui` | `Partial<{ root: UiPartValue; trigger: UiPartValue; panel: UiPartValue; body: UiPartValue; }> \| undefined` |  | Class and style overrides for each part.
`open` | `boolean \| undefined` | `false` | Whether the panel is open.

## Slots

Name | Type | Description
--- | --- | ---
`trigger` | `{ open: boolean; }` | Toggle control: clicks toggle the panel, and its first button receives `aria-expanded` and `aria-controls`.
`default` | `any` | Panel content.

## Events

Name | Type | Description
--- | --- | ---
`update:open` | `[value: boolean]` | Fires when `open` changes (`v-model:open`).

## Exposed

Name | Type | Description
--- | --- | ---
`el` | `HTMLElement \| null` | Root element.
`panelEl` | `HTMLElement \| null` | Panel element, always mounted. 


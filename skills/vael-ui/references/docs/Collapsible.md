# Collapsible

A single show/hide section for content that is optional or secondary.

```ts
import { Collapsible } from 'vael-ui' // or 'vael-ui/vapor'
```

## Props

Name | Type | Default | Description
--- | --- | --- | ---
`disabled` | `boolean \| undefined` | `false` | Blocks toggling and marks the trigger `aria-disabled`.
`motion` | `CollapsibleMotion \| undefined` | `true` | How it moves: its appearance (`{ initial: { panel: { opacity: 0 } } }`) and its states (`{ states: { open: { trigger: { opacity: 0.7 } } } }`). `false` skips the height transition; use the exposed `panelEl` for your own.
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
`parts` | `Record<string, HTMLElement \| HTMLElement[] \| null>` | Its elements by part name, for your own animation; a repeated part is an array.

## Motion

`motion` takes `CollapsibleMotion` (exported from `'vael-ui'`); `false` turns it off. Collapsible: its appearance and states. */ type CollapsibleHook = MotionHook< CollapsiblePart \| 'body', { signal: AbortSignal; reducedMotion: boolean } > /** Collapsible: its appearance and states, and your own opening and closing. With a hook its height no longer eases: `enter` runs once the panel is open (animate it from closed), and `leave` before it closes, which waits for what it returns.

- Parts: `root`, `trigger`, `panel`
- States: `open`, `disabled`
- Keys: `spring`, `duration`, `easing`, `initial`, `states`, `replay`, `appear`, `onAfterEnter`

See the [Motion guide](../guides/motion.md) for what each key does.


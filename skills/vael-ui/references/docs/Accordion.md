# Accordion

A stack of collapsible sections where opening one can close the others.

```ts
import { Accordion } from 'vael-ui' // or 'vael-ui/vapor'
```

## Props

Name | Type | Default | Description
--- | --- | --- | ---
`multiple` | `boolean \| undefined` | false | 
`collapsible` | `boolean \| undefined` | true | Whether the last open item can close, leaving none open.
`motionCss` | `boolean \| undefined` | true | `false` skips transitions; use exposed `panelEl`/`open` for custom motion.
`ui` | `Partial<{ root: UiPartValue; }> \| undefined` |  | 
`value` | `string \| string[] \| null \| undefined` | null | 

## Slots

Name | Type | Description
--- | --- | ---
`default` | `{}` | 

## Events

Name | Type | Description
--- | --- | ---
`change` | `[value: string \| string[] \| null]` | 
`update:value` | `[value: string \| string[] \| null]` | 

## Exposed

Name | Type | Description
--- | --- | ---
`el` | `HTMLElement \| null` | 


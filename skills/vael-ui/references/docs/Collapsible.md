# Collapsible

A single show/hide section for content that is optional or secondary.

```ts
import { Collapsible } from 'vael-ui' // or 'vael-ui/vapor'
```

## Props

Name | Type | Default | Description
--- | --- | --- | ---
`disabled` | `boolean \| undefined` | false | 
`motionCss` | `boolean \| undefined` | true | 
`ui` | `Partial<{ root: UiPartValue; trigger: UiPartValue; panel: UiPartValue; }> \| undefined` |  | 
`open` | `boolean \| undefined` | false | 

## Slots

Name | Type | Description
--- | --- | ---
`trigger` | `{ open: boolean; }` | 
`default` | `any` | 

## Events

Name | Type | Description
--- | --- | ---
`update:open` | `[value: boolean]` | 

## Exposed

Name | Type | Description
--- | --- | ---
`el` | `HTMLElement \| null` | 
`panelEl` | `HTMLElement \| null` | 


# Resizable

A draggable handle for letting the user resize a panel between a min and max size.

```ts
import { Resizable } from 'vael-ui' // or 'vael-ui/vapor'
```

## Props

Name | Type | Default | Description
--- | --- | --- | ---
`min` | `number \| undefined` | 0 | 
`max` | `number \| undefined` | Infinity | 
`direction` | `ResizeDirection \| undefined` | "horizontal" | 
`edge` | `ResizeEdge \| undefined` | "end" | 
`disabled` | `boolean \| undefined` | false | 
`ariaLabel` | `string \| undefined` | "Resize" | 
`ui` | `Partial<{ root: UiPartValue; handle: UiPartValue; }> \| undefined` |  | 
`size` | `number` |  | 

## Slots

Name | Type | Description
--- | --- | ---
`default` | `any` | 
`handle` | `any` | 

## Events

Name | Type | Description
--- | --- | ---
`update:size` | `[value: number]` | 

## Exposed

Name | Type | Description
--- | --- | ---
`el` | `HTMLElement \| null` | 
`handleEl` | `HTMLElement \| null` | 
`isDragging` | `boolean` | 


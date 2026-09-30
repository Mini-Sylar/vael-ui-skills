# Resizable

A draggable handle for letting the user resize a panel between a min and max size.

```ts
import { Resizable } from 'vael-ui' // or 'vael-ui/vapor'
```

## Props

Name | Type | Default | Description
--- | --- | --- | ---
`min` | `number \| undefined` | `0` | Smallest size in pixels.
`max` | `number \| undefined` | `Infinity` | Largest size in pixels.
`direction` | `ResizeDirection \| undefined` | `'horizontal'` | Axis the panel resizes along: `'horizontal'` sets its width, `'vertical'` its height.
`edge` | `ResizeEdge \| undefined` | `'end'` | Which edge the handle sits on. `'start'` flips the drag so moving toward the panel grows it.
`disabled` | `boolean \| undefined` | `false` | Disables dragging and keyboard resizing, and removes the handle from the tab order.
`ariaLabel` | `string \| undefined` | `'Resize'` | Accessible label for the handle.
`ui` | `Partial<{ root: UiPartValue; handle: UiPartValue; }> \| undefined` |  | Class and style overrides for each part.
`size` | `number` |  | Panel size in pixels along `direction`. Overshoots `min` or `max` elastically mid-drag, then settles within them.

## Slots

Name | Type | Description
--- | --- | ---
`default` | `any` | Panel content.
`handle` | `any` | Custom content inside the drag handle.

## Events

Name | Type | Description
--- | --- | ---
`update:size` | `[value: number]` | Fires when `size` changes (`v-model:size`).

## Exposed

Name | Type | Description
--- | --- | ---
`el` | `HTMLElement \| null` | Root element.
`handleEl` | `HTMLElement \| null` | Drag handle element.
`isDragging` | `boolean` | Whether you're dragging the handle with a pointer. 


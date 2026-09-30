# ButtonGroup

```ts
import { ButtonGroup } from 'vael-ui' // or 'vael-ui/vapor'
```

## Props

Name | Type | Default | Description
--- | --- | --- | ---
`orientation` | `"horizontal" \| "vertical" \| undefined` | `'horizontal'` | Whether the buttons sit in a row or a column.
`ariaLabel` | `string \| undefined` |  | Accessible name for the group.
`ui` | `Partial<{ root: UiPartValue; }> \| undefined` |  | Class and style overrides for each part.

## Slots

Name | Type | Description
--- | --- | ---
`default` | `any` | The grouped buttons.

## Events

_None._

## Exposed

Name | Type | Description
--- | --- | ---
`el` | `HTMLElement \| null` | Root element. 


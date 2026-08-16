# Separator

A thin line (or gap) for dividing content into visually distinct groups.

```ts
import { Separator } from 'vael-ui' // or 'vael-ui/vapor'
```

## Props

Name | Type | Default | Description
--- | --- | --- | ---
`orientation` | `"horizontal" \| "vertical" \| undefined` | "horizontal" | 
`ui` | `Partial<{ root: UiPartValue; line: UiPartValue; text: UiPartValue; }> \| undefined` |  | 

## Slots

Name | Type | Description
--- | --- | ---
`default` | `any` | Optional label centered in the line, e.g. "OR". Plain divider without it.

## Events

_None._

## Exposed

Name | Type | Description
--- | --- | ---
`el` | `HTMLElement \| null` | 


# Avatar

A circular (or square) image or initials, representing a person or entity.

```ts
import { Avatar } from 'vael-ui' // or 'vael-ui/vapor'
```

## Props

Name | Type | Default | Description
--- | --- | --- | ---
`src` | `string \| undefined` |  | 
`alt` | `string \| undefined` |  | 
`name` | `string \| undefined` |  | 
`size` | `"md" \| "sm" \| "lg" \| undefined` | "md" | 
`shape` | `"circle" \| "square" \| undefined` | "circle" | 
`badgePlacement` | `"top-end" \| "top-start" \| "bottom-start" \| "bottom-end" \| undefined` | "bottom-end" | 
`ui` | `Partial<{ root: UiPartValue; image: UiPartValue; fallback: UiPartValue; badge: UiPartValue; }> \| undefined` |  | 

## Slots

Name | Type | Description
--- | --- | ---
`default` | `any` | 
`badge` | `any` | 

## Events

Name | Type | Description
--- | --- | ---
`load` | `[event: Event]` | 
`error` | `[event: Event]` | 

## Exposed

Name | Type | Description
--- | --- | ---
`el` | `HTMLElement \| null` | 
`imgEl` | `HTMLImageElement \| null` | 


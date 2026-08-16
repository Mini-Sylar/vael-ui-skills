# Skeleton

A placeholder shape that mimics real content while it is still loading.

```ts
import { Skeleton } from 'vael-ui' // or 'vael-ui/vapor'
```

## Props

Name | Type | Default | Description
--- | --- | --- | ---
`variant` | `"text" \| "rect" \| "circle" \| undefined` | "text" | `text` (default): 1em-tall rounded line. `circle`: round, aspect-ratio 1. `rect`: `--ui-radius` corners, sized by content or `ui.root`.
`animated` | `boolean \| undefined` | true | 
`ui` | `Partial<{ root: UiPartValue; }> \| undefined` |  | 

## Slots

Name | Type | Description
--- | --- | ---
`default` | `{}` | 

## Events

_None._

## Exposed

Name | Type | Description
--- | --- | ---
`el` | `HTMLElement \| null` | 


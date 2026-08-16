# Badge

A small dot, count, or label overlaid on the corner of another element.

```ts
import { Badge } from 'vael-ui' // or 'vael-ui/vapor'
```

## Props

Name | Type | Default | Description
--- | --- | --- | ---
`variant` | `"primary" \| "danger" \| "muted" \| "success" \| "warning" \| "info" \| undefined` | "primary" | 
`count` | `number \| undefined` |  | 
`max` | `number \| undefined` | 99 | Counts above this render as `"${max}+"`.
`dot` | `boolean \| undefined` | false | Minimal size, no content — a plain presence dot.
`animated` | `boolean \| undefined` | true | `false` drops the built-in count-change animation — use when driving your own animation instead.
`ui` | `Partial<{ root: UiPartValue; }> \| undefined` |  | 

## Slots

Name | Type | Description
--- | --- | ---
`default` | `any` | Overrides `count` entirely — anything you render here wins.

## Events

_None._

## Exposed

Name | Type | Description
--- | --- | ---
`el` | `HTMLElement \| null` | 


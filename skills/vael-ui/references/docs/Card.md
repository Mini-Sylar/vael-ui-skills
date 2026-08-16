# Card

A bordered container for grouping related content, with optional title and actions.

```ts
import { Card } from 'vael-ui' // or 'vael-ui/vapor'
```

## Props

Name | Type | Default | Description
--- | --- | --- | ---
`title` | `string \| undefined` |  | Default header title; ignored when `#header` is used.
`description` | `string \| undefined` |  | Default header description; ignored when `#header` is used.
`as` | `string \| undefined` | "div" | Root tag — `'a'`/`'button'` for a fully interactive card.
`interactive` | `boolean \| undefined` | false | Hover/press affordance. Implied (always on) when `as` is `'a'` or `'button'`.
`ui` | `Partial<{ root: UiPartValue; header: UiPartValue; title: UiPartValue; description: UiPartValue; body: UiPartValue; footer: UiPartValue; }> \| undefined` |  | 

## Slots

Name | Type | Description
--- | --- | ---
`default` | `any` | 
`header` | `any` | Replaces the default title/description header.
`footer` | `any` | 

## Events

_None._

## Exposed

Name | Type | Description
--- | --- | ---
`el` | `HTMLElement \| null` | 


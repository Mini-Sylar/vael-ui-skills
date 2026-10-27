# Breadcrumb

A trail of links from the top of the site down to the current page.

```ts
import { Breadcrumb } from 'vael-ui' // or 'vael-ui/vapor'
```

## Props

Name | Type | Default | Description
--- | --- | --- | ---
`items` | `readonly T[] \| undefined` |  | Crumbs to render, with separators added between them. Omit to compose children in the default slot.
`ariaLabel` | `string \| undefined` |  | Overrides the default localized "Breadcrumb" nav landmark label.
`wrap` | `boolean \| undefined` | `false` | Wraps crumbs onto multiple lines instead of scrolling one line horizontally on overflow.
`motion` | `BreadcrumbMotion \| undefined` |  | How it moves: its appearance, crumb by crumb (`{ initial: { item: { opacity: 0, x: -4 } }, stagger: 0.03 }`). Crumbs a new path adds or removes come and go the same way. `false` turns its own animation off.
`ui` | `Partial<{ root: UiPartValue; list: UiPartValue; icon: UiPartValue; }> \| undefined` |  | Class and style overrides for each part.

## Slots

Name | Type | Description
--- | --- | ---
`default` | `any` | `BreadcrumbItem` and `BreadcrumbSeparator` children that you interleave. Has no effect when you pass `items`.
`item` | `{ item: T; index: number; }` | Custom label content for each crumb when you pass `items`. Falls back to plain text.

## Events

_None._

## Exposed

Name | Type | Description
--- | --- | ---
`el` | `HTMLElement \| null` | Root element.
`parts` | `Record<string, HTMLElement \| HTMLElement[] \| null>` | Its elements by part name, for your own animation; a repeated part is an array.

## Motion

`motion` takes `BreadcrumbMotion` (exported from `'vael-ui'`); `false` turns it off. Breadcrumb: its appearance, crumb by crumb.

- Parts: `root`, `item`
- Keys: `spring`, `duration`, `easing`, `initial`, `exit`, `enter`, `leave`, `stagger`, `leaving`, `appear`, `onAfterEnter`, `onAfterLeave`

See the [Motion guide](../guides/motion.md) for what each key does.

## BreadcrumbItem

### Props

Name | Type | Default | Description
--- | --- | --- | ---
`as` | `string \| undefined` | `'a'` | Root tag for the link, such as `'a'` or a router's link component. Has no effect when `current` is `true`.
`current` | `boolean \| undefined` | `false` | Renders the last, active crumb: plain text with `aria-current="page"` instead of a link.
`ui` | `Partial<{ item: UiPartValue; link: UiPartValue; current: UiPartValue; }> \| undefined` |  | Class and style overrides for each part.

### Slots

Name | Type | Description
--- | --- | ---
`default` | `any` | Crumb label.

### Events

_None._

### Exposed

Name | Type | Description
--- | --- | ---
`el` | `HTMLElement \| null` | Root element.

## BreadcrumbSeparator

### Props

Name | Type | Default | Description
--- | --- | --- | ---
`ui` | `Partial<{ root: UiPartValue; icon: UiPartValue; }> \| undefined` |  | Class and style overrides for each part.

### Slots

Name | Type | Description
--- | --- | ---
`default` | `any` | Replaces the default chevron icon.

### Events

_None._

### Exposed

Name | Type | Description
--- | --- | ---
`el` | `HTMLElement \| null` | Root element.


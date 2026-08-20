# Breadcrumb

```ts
import { Breadcrumb } from 'vael-ui' // or 'vael-ui/vapor'
```

## Props

Name | Type | Default | Description
--- | --- | --- | ---
`items` | `readonly T[] \| undefined` |  | Data-driven alternative to composing `BreadcrumbItem`/`BreadcrumbSeparator` yourself in the default slot. Omit to use the default slot instead — the two are mutually exclusive per instance.
`ariaLabel` | `string \| undefined` |  | Overrides the default localized "Breadcrumb" nav landmark label.
`wrap` | `boolean \| undefined` | false | `false` (default): a single line that scrolls horizontally once it overflows, edge-faded like Dialog/Select. `true`: wraps onto multiple lines instead.
`ui` | `Partial<{ root: UiPartValue; list: UiPartValue; }> \| undefined` |  | 

## Slots

Name | Type | Description
--- | --- | ---
`default` | `any` | `BreadcrumbItem` and `BreadcrumbSeparator` children, interleaved by the caller. Ignored when `items` is set.
`item` | `{ item: T; index: number; }` | Overrides one crumb's label content when using `items`. Falls back to plain text.

## Events

_None._

## Exposed

Name | Type | Description
--- | --- | ---
`el` | `unknown` | Type inference unavailable — vue-component-meta cannot resolve defineExpose on this generic component.


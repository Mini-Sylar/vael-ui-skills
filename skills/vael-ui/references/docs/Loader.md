# Loader

A small spinner for indicating that something is in progress.

```ts
import { Loader } from 'vael-ui' // or 'vael-ui/vapor'
```

## Props

Name | Type | Default | Description
--- | --- | --- | ---
`size` | `string \| undefined` |  | Any CSS length. Sets the root's font-size — the ring is sized in `em`, so it scales with it.
`label` | `string \| undefined` |  | Renders role="status" with visually-hidden text as the accessible name. Omit to make the loader aria-hidden.
`ui` | `Partial<{ root: UiPartValue; }> \| undefined` |  | 

## Slots

_None._

## Events

_None._

## Exposed

Name | Type | Description
--- | --- | ---
`el` | `HTMLElement \| null` | 


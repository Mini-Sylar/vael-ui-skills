# Button

A clickable action with variants, sizes, loading states, and icon slots built in.

```ts
import { Button } from 'vael-ui' // or 'vael-ui/vapor'
```

## Props

Name | Type | Default | Description
--- | --- | --- | ---
`loading` | `boolean \| "auto" \| undefined` | `false` | `true` and `false` set the loading state directly. With `'auto'`, the button stays loading until every overlapping promise your `@click` handler returns settles.
`disabled` | `boolean \| undefined` | `false` | Disables the button and blocks interaction.
`variant` | `ButtonVariant \| undefined` | `'primary'` | Visual style of the button.
`size` | `ButtonSize \| undefined` | `'md'` | Button size.
`loader` | `ButtonLoaderPlacement \| undefined` | `'overlay'` | `'overlay'` centers the loader over the fading content; `'inline'` slides a spinner in at the start.
`icon` | `boolean \| undefined` | `false` | Renders a square icon-only button; pair it with an `aria-label`.
`pill` | `boolean \| undefined` | `false` | Gives the button a fully rounded pill shape.
`block` | `boolean \| undefined` | `false` | Stretches the button to its container's full inline size.
`type` | `"button" \| "submit" \| "reset" \| undefined` | `'button'` | Native `type` attribute; applied only when `as` is `'button'`.
`as` | `string \| undefined` | `'button'` | Root tag, such as `as="a"` for a link styled as a button.
`badgePlacement` | `"top-start" \| "top-end" \| "bottom-start" \| "bottom-end" \| undefined` |  | Where the `#badge` slot sits (`'top-end'` when unset). Setting it reserves the wrapper up front, so a `v-if`'d `#badge` keeps a stable DOM position.
`ui` | `Partial<{ root: UiPartValue; leading: UiPartValue; trailing: UiPartValue; content: UiPartValue; label: UiPartValue; badge: UiPartValue; }> \| undefined` |  | Class and style overrides for each part.

## Slots

Name | Type | Description
--- | --- | ---
`default` | `{ loading: boolean; el: HTMLElement \| null; run: <T>(fn: () => T \| Promise<T>) => Promise<T>; }` | Button label; receives the loading state, the root element and `run`.
`loader` | `{ loading: boolean; el: HTMLElement \| null; }` | Custom loader visuals; the library still handles placement and the crossfade.
`leading` | `any` | Icon before the label, in an optically aligned `1em` box.
`trailing` | `any` | Icon after the label, in an optically aligned `1em` box.
`badge` | `any` | Badge content, placed in a wrapper the library positions.

## Events

_None._

## Exposed

Name | Type | Description
--- | --- | ---
`el` | `HTMLElement \| null` | Root element (the button itself, even when a badge wrapper is rendered).
`loading` | `boolean` | Whether the button is loading, from the `loading` prop or `'auto'` tracking.
`run` | `<T>(fn: () => T \| Promise<T>) => Promise<T>` | Runs `fn` with the loading state shown until it settles. 


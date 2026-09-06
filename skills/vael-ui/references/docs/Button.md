# Button

A clickable action with variants, sizes, loading states, and icon slots built in.

```ts
import { Button } from 'vael-ui' // or 'vael-ui/vapor'
```

## Props

Name | Type | Default | Description
--- | --- | --- | ---
`loading` | `boolean \| "auto" \| undefined` | false | `false` (default) / `true`: fully controlled. `'auto'`: opt into promise-based loading — `@click` returning a promise drives it, finishing only once the last overlapping promise resolves.
`disabled` | `boolean \| undefined` | false | 
`variant` | `ButtonVariant \| undefined` | "primary" | 
`size` | `ButtonSize \| undefined` | "md" | 
`loader` | `ButtonLoaderPlacement \| undefined` | "overlay" | `overlay` (default): loader centered over fading content. `inline`: spinner slides in at start.
`icon` | `boolean \| undefined` | false | Square icon-only button; pair with an aria-label.
`pill` | `boolean \| undefined` | false | Fully rounded capsule/pill shape.
`block` | `boolean \| undefined` | false | Stretch to container's full inline size.
`type` | `"button" \| "submit" \| "reset" \| undefined` | "button" | 
`as` | `string \| undefined` | "button" | Root tag (e.g. `as="a"` for a link styled as button).
`badgePlacement` | `"top-end" \| "top-start" \| "bottom-start" \| "bottom-end" \| undefined` | "top-end" | Where the `#badge` slot wrapper sits relative to the button.
`ui` | `Partial<{ root: UiPartValue; leading: UiPartValue; trailing: UiPartValue; content: UiPartValue; label: UiPartValue; badge: UiPartValue; }> \| undefined` |  | `leading`/`trailing` wrap the matching slots and carry the default gap to the label — zero it there instead of margin-hacking the slot content. `content`/`label` reach the default-slot wrapper and the label itself (`content > label`, `label` is `display: inline-block` by default — override for e.g. an icon-above-label stacked layout).

## Slots

Name | Type | Description
--- | --- | ---
`default` | `{ loading: boolean; el: HTMLElement \| null; run: <T>(fn: () => T \| Promise<T>) => Promise<T>; }` | 
`loader` | `{ loading: boolean; el: HTMLElement \| null; }` | Custom loader visuals (placement and crossfade are library-owned).
`leading` | `any` | Icon before the label (1em box, optically aligned).
`trailing` | `any` | Icon after the label (1em box, optically aligned).
`badge` | `any` | Badge in library-positioned wrapper.

## Events

_None._

## Exposed

Name | Type | Description
--- | --- | ---
`el` | `HTMLElement \| null` | 
`run` | `<T>(fn: () => T \| Promise<T>) => Promise<T>` | 


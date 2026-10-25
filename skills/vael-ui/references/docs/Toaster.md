# Toaster

Renders the notifications created by toast(). Mount it once, anywhere in your app.

```ts
import { Toaster } from 'vael-ui' // or 'vael-ui/vapor'
```

## Props

Name | Type | Default | Description
--- | --- | --- | ---
`name` | `string \| undefined` |  | Shows only toasts sent to this name. Without one, it shows toasts that have no `toaster`.
`position` | `'bottom-right' \| 'top-left' \| 'top-center' \| 'top-right' \| 'bottom-left' \| 'bottom-center' \| undefined` | `'bottom-right'` | Screen corner or edge the toasts stack from.
`maxVisible` | `number \| undefined` | `4` | Max toasts shown at once. Extra toasts wait off-screen, timers paused, until a slot opens.
`expand` | `boolean \| undefined` | `false` | Keeps the stack open as a list, so every toast stays readable. Timers keep running, and hover or focus still pauses them.
`gap` | `number \| undefined` | `10` | Spacing between stacked cards, in pixels.
`teleportTo` | `string \| HTMLElement \| undefined` | `'body'` | Teleport target: a CSS selector or element. Wins over `container` either way.
`container` | `DOMTarget \| undefined` |  | Scopes the stack to an element instead of the viewport: it teleports there and sits in that element's corner, so give the element `position: relative`. Pair with `name` to send only some toasts there. Omit it for a page-level toaster.
`motion` | `ToasterMotion \| undefined` | `true` | How toasts come and go. `false` shows and removes them at once. An object animates them your way: values for the `toast` part, or `enter` / `leave` hooks for your own library. The stack places them either way.
`ui` | `Partial<{ root: UiPartValue; toast: UiPartValue; icon: UiPartValue; content: UiPartValue; title: UiPartValue; description: UiPartValue; action: UiPartValue; close: UiPartValue; }> \| undefined` |  | Class and style overrides for each part.

## Slots

Name | Type | Description
--- | --- | ---
`default` | `{ entry: ToastEntry; dismiss: () => void; depth: number; expanded: boolean; }` | Replaces a toast's inner markup; the toaster still handles its positioning, stacking and swipe.

## Events

_None._

## Exposed

Name | Type | Description
--- | --- | ---
`toasterEl` | `HTMLElement \| null` | The toast list (`<ol>`) element, inside the live region.

## Motion

`motion` takes `ToasterMotion` (exported from `'vael-ui'`); `false` turns it off. Toaster's `motion`: values or hooks for its parts (ToasterPart), its states (`loading`, `success`, `error`, `warning`, `info`), how its items come and go (`stagger`, `leaving`, `appear`).

- Parts: `root`, `toast`, `icon`, `content`, `title`, `description`, `action`, `close`
- States: `loading`, `success`, `error`, `warning`, `info`
- Keys: `spring`, `duration`, `easing`, `initial`, `states`, `exit`, `enter`, `leave`, `stagger`, `leaving`, `appear`

See the [Motion guide](../guides/motion.md) for what each key does.


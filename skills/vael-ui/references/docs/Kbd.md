# Kbd

Styles a keyboard shortcut so it reads like a physical key.

```ts
import { Kbd } from 'vael-ui' // or 'vael-ui/vapor'
```

## Props

Name | Type | Default | Description
--- | --- | --- | ---
`ui` | `Partial<{ root: UiPartValue; }> \| undefined` |  | Class and style overrides for each part.

## Slots

Name | Type | Description
--- | --- | ---
`default` | `any` | Key or shortcut text, e.g. `⌘` or `Esc`.

## Events

_None._

## Exposed

Name | Type | Description
--- | --- | ---
`el` | `HTMLElement \| null` | Root element. 


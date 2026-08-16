# AvatarGroup

```ts
import { AvatarGroup } from 'vael-ui' // or 'vael-ui/vapor'
```

## Props

Name | Type | Default | Description
--- | --- | --- | ---
`size` | `"md" \| "sm" \| "lg" \| undefined` | "md" | Sizes the generated overflow avatar; slotted `Avatar`s keep their own `size` prop.
`overflowCount` | `number \| undefined` | 0 | Count of items NOT rendered as slotted `Avatar`s — the caller decides truncation, this only displays the remainder as "+N". 0 (default) renders nothing.
`hoverLift` | `boolean \| undefined` | false | Lifts an avatar on hover to reveal it above its neighbors. Off by default.
`ui` | `Partial<{ root: UiPartValue; overflow: UiPartValue; }> \| undefined` |  | 

## Slots

Name | Type | Description
--- | --- | ---
`default` | `any` | 
`overflow` | `{ count: number; }` | Replaces the default "+N" content of the generated overflow avatar.

## Events

_None._

## Exposed

Name | Type | Description
--- | --- | ---
`el` | `HTMLElement \| null` | 


# AvatarGroup

```ts
import { AvatarGroup } from 'vael-ui' // or 'vael-ui/vapor'
```

## Props

Name | Type | Default | Description
--- | --- | --- | ---
`size` | `"md" \| "sm" \| "lg" \| undefined` | `'md'` | Sizes the generated overflow avatar; slotted `Avatar`s keep their own `size` prop.
`overflowCount` | `number \| undefined` | `0` | Number of items you didn't render as slotted `Avatar`s, shown as "+N". You decide the truncation; `0` renders nothing.
`hoverLift` | `boolean \| undefined` | `false` | Lifts an avatar on hover to reveal it above its neighbors.
`ui` | `Partial<{ root: UiPartValue; overflow: UiPartValue; }> \| undefined` |  | Class and style overrides for each part.

## Slots

Name | Type | Description
--- | --- | ---
`default` | `any` | The `Avatar`s to stack.
`overflow` | `{ count: number; }` | Replaces the default "+N" content of the generated overflow avatar.

## Events

_None._

## Exposed

Name | Type | Description
--- | --- | ---
`el` | `HTMLElement \| null` | Root element. 


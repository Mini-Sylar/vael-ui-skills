# AvatarGroup

Stacks several avatars into one overlapping row, with a count for the ones that don’t fit.

```ts
import { AvatarGroup } from 'vael-ui' // or 'vael-ui/vapor'
```

## Props

Name | Type | Default | Description
--- | --- | --- | ---
`size` | `"sm" \| "md" \| "lg" \| undefined` | `'md'` | Sizes the generated overflow avatar; slotted `Avatar`s keep their own `size` prop.
`overflowCount` | `number \| undefined` | `0` | Number of items you didn't render as slotted `Avatar`s, shown as "+N". You decide the truncation; `0` renders nothing.
`hoverLift` | `boolean \| undefined` | `false` | Lifts an avatar on hover to reveal it above its neighbors.
`motion` | `AvatarGroupMotion \| undefined` |  | How it moves: its appearance, avatar by avatar (`{ initial: { avatar: { opacity: 0, x: -8 } }, stagger: 0.04 }`). Avatars you add or remove come and go the same way (key them). `false` turns its own animation off.
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
`parts` | `Record<string, HTMLElement \| HTMLElement[] \| null>` | Its elements by part name, for your own animation; a repeated part is an array.

## Motion

`motion` takes `AvatarGroupMotion` (exported from `'vael-ui'`); `false` turns it off. AvatarGroup: its appearance, avatar by avatar.

- Parts: `root`, `avatar`
- Keys: `spring`, `duration`, `easing`, `initial`, `exit`, `enter`, `leave`, `stagger`, `leaving`, `appear`

See the [Motion guide](../guides/motion.md) for what each key does.


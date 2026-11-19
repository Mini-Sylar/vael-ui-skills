# useHaptic

Plays haptic feedback on phones that support it: `navigator.vibrate` on Android, and on iOS (Safari 17.4+) the tick a native switch plays. Desktops do nothing, and `isSupported` tells you which you have. `intensity` scales every pulse. `ConfigProvider`'s `haptics` sets it app-wide: `false` silences every call, yours included, for a "turn off haptics" setting, and `true` or `{ intensity }` also turns on vael-ui's own haptics. Call it from a user gesture, and keep it for moments that commit something.

```ts
import { useHaptic } from 'vael-ui' // or 'vael-ui/vapor'
```

## Example

```ts
import { useHaptic } from 'vael-ui'

const haptic = useHaptic({ intensity: 1.5 })

function onSave() {
  save()
  haptic.notification('success')
}

// App-wide, in your root:
// <ConfigProvider :haptics="{ intensity: 1.2 }"> or :haptics="userWantsHaptics"
```

## Parameters

Name | Type | Description
--- | --- | ---
`intensity` | `number` | Scales every pulse: `0.5` is softer, `2` stronger. Default `1`. Multiplies with ConfigProvider's `haptics.intensity`. On Android it lengthens each pulse; iOS plays one fixed-strength tick per pulse.

## Returns

Name | Type | Description
--- | --- | ---
`isSupported` | `boolean` | Whether this device plays haptics.
`selection` | `() => void` | Plays the lightest tick, for a choice changing.
`impact` | `(weight?: 'light' \| 'medium' \| 'heavy') => void` | Plays a bump, heavier for bigger moments. Default `'medium'`.
`notification` | `(type: 'success' \| 'warning' \| 'error') => void` | Plays a short pattern for an outcome.
`vibrate` | `(pattern: number \| number[]) => void` | Plays your own pattern in ms, vibration and pause alternating. iOS plays a tick per pulse.


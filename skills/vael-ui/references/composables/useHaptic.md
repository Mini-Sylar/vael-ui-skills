# useHaptic

Haptic feedback on phones that offer it: `navigator.vibrate` on Android, and on iOS (Safari 17.4+) the tick a native switch plays. Desktops do nothing, and `isSupported` tells you which you have. `selection()` is the lightest tick, for a choice changing; `impact('light' | 'medium' | 'heavy')` is a bump, heavier for bigger moments; `notification('success' | 'warning' | 'error')` plays a short pattern for an outcome; `vibrate(pattern)` plays your own. `intensity` scales every pulse. `ConfigProvider`'s `haptics` sets it app-wide: `false` silences every call, yours included, which suits a "turn off haptics" setting; `true` or `{ intensity }` also turns on vael-ui's own components. Call it from a user gesture, and keep it for moments that commit something.

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
`intensity` | `number` | Scales every pulse: `0.5` is softer, `2` stronger. Multiplies with ConfigProvider's `haptics.intensity`. On Android it lengthens each pulse; iOS plays one fixed-strength tick per pulse. Default `1`.

## Returns

Name | Type | Description
--- | --- | ---
`isSupported` | `boolean` | Whether this device plays haptics.
`selection` | `() => void` | The lightest tick, for a choice changing.
`impact` | `(weight?: 'light' \| 'medium' \| 'heavy') => void` | A bump. Default `'medium'`.
`notification` | `(type: 'success' \| 'warning' \| 'error') => void` | A short pattern for an outcome.
`vibrate` | `(pattern: number \| number[]) => void` | Your own pattern in ms, vibration and pause alternating. iOS plays a tick per pulse.

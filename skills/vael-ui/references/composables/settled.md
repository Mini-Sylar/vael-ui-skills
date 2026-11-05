# settled

Resolves once nothing vael-ui started is still animating, for tests: open something, `await settled()`, then assert. Pass an element to wait only for what animates on it and inside it, such as a field's invalid shake: `await settled(field.value?.el)`. With nothing playing (reduced motion, `motion="false"`, or no element) it resolves at once, and it never waits for an endless loop. In app code, put a callback on what animates instead, such as `onAfterEnter` or `onAfterLeave`.

```ts
import { settled } from 'vael-ui/motion' // either build
```

## Example

```ts
import { settled } from 'vael-ui/motion'
import { expect, test } from 'vitest'
import { render } from 'vitest-browser-vue'
import App from './App.vue'

test('the dialog opens', async () => {
  const screen = render(App)
  await screen.getByRole('button', { name: 'Open' }).click()
  await settled()
  await expect.element(screen.getByRole('dialog')).toBeVisible()
})
```

## Parameters

Name | Type | Description
--- | --- | ---
`scope` | `Element \| null` | Waits only for what animates on this element and inside it. Leave it out to wait for everything vael-ui started.

## Returns

Name | Type | Description
--- | --- | ---
`promise` | `Promise<void>` | Resolves once those animations have finished.


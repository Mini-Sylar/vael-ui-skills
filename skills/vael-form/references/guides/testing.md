# Testing forms

Patterns taken from vael-form's own suite (`src/*.test.ts` run in Node, `tests/*.test.ts` in a browser via Vitest browser mode, in VDOM, Vapor and Vapor-in-VDOM).

## The form alone (no DOM)

`useForm` works outside a component. Make it inside an `effectScope` so its computeds and watchers have an owner:

```ts
import { effectScope, nextTick } from 'vue'
import { z } from 'zod'
import { useForm } from 'vael-form'

const make = <T>(fn: () => T) => effectScope().run(fn)!
const flush = async () => {
  await nextTick()
  await new Promise((r) => setTimeout(r, 5))
}

it('validates and submits', async () => {
  const onSubmit = vi.fn()
  const form = make(() =>
    useForm({ schema: z.object({ name: z.string().min(2, 'Too short') }), defaultValues: { name: '' }, onSubmit }),
  )

  expect(await form.handleSubmit()).toBe(false) // returns whether it was valid
  expect(form.field('name').error).toBe('Too short') // a submit shows everything

  form.field('name').value = 'Ada'
  await flush()
  expect(await form.handleSubmit()).toBe(true)
  expect(onSubmit).toHaveBeenCalledWith({ name: 'Ada' }, expect.anything())
})
```

- Validation is lazy: reading errors or state starts it. `await form.validate()` (or `validate('group')`) is the dependable way to get a result; reading `form.valid.value` then `flush()` also works.
- `form.field('x').value = …` and `form.setValue('x', …)` count as user edits; `setValues` and `reset` don't (so `watch` handlers don't fire).
- Visibility follows `showErrors`. Before a submit, a field's error only shows once it's been edited and left: call `form.field('x').touch()`, or test with `showErrors: 'input'`.
- `rules` load a lazy chunk and are debounced: `await form.validate()` waits for them. With fake timers, advance by the `debounce` first.
- Translated messages: pass `i18n` (`useForm({ i18n: i18n.global })`) or install `formPlugin` on the test app. The catalog readers are a lazy chunk: the first read may show the library's own text, so `flush()` before asserting translated text.

## Components (DOM)

Drive real inputs: set the value and dispatch `input`; leave a field with a **bubbling** `focusout` (that's what `attrs.onFocusout` listens to; `blur` doesn't bubble).

```ts
async function type(input: HTMLInputElement, value: string) {
  input.value = value
  input.dispatchEvent(new Event('input', { bubbles: true }))
  await flush()
}
async function leave(input: HTMLElement) {
  input.dispatchEvent(new FocusEvent('blur'))
  input.dispatchEvent(new FocusEvent('focusout', { bubbles: true }))
  await flush()
}
async function flush() {
  for (let i = 0; i < 3; i++) {
    await nextTick()
    await Promise.resolve()
  }
}

it('shows the error once the field is left, wired to the input', async () => {
  const email = screen.getByLabelText('Email') as HTMLInputElement
  await type(email, 'nope')
  expect(email.hasAttribute('aria-invalid')).toBe(false) // 'edited': not yet
  await leave(email)
  expect(email.getAttribute('aria-invalid')).toBe('true')
  const describedBy = email.getAttribute('aria-describedby')!
  expect(document.getElementById(describedBy)?.textContent?.trim()).toBe('Invalid email')
})

it('focuses the first invalid field on submit', async () => {
  form.dispatchEvent(new Event('submit', { cancelable: true, bubbles: true }))
  await flush()
  expect(document.activeElement).toBe(screen.getByLabelText('Email'))
})
```

- A synthetic `focusout` with no pointer held marks the field left at once. With a real pointer press (`userEvent.click` on another element) the touch waits for the pointer release and a macrotask: flush a `setTimeout(0)` too.
- A field still holding a browser-autofilled value is not marked left by focusout; jsdom/happy-dom don't autofill, so this rarely matters in tests.
- `await form.handleSubmit()` without an event (from code) focuses the first invalid field on the page by id, or by the error id in `aria-describedby`.
- The submit context's `element` and `submitter` are only set when a real submit event reaches `handleSubmit`.
- Ids are `${formId}-${path}`; pass `id: 'test'` to `useForm` for predictable ids outside components, or query by label instead.
- vael-ui components animate: `await settled()` from `vael-ui/motion`, or wrap the fixture in `<ConfigProvider :motion="false">`.

## SSR

Render with `renderToString(createSSRApp(App))`: expect `name="email"`, an id ending in `-email`, the default value, and no error text or `aria-invalid`. Rendering twice gives the same ids.

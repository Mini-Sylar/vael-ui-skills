# Composable reference index

## Overlays & Confirmation

- [confirmAction](./confirmAction.md): One function for async-aware confirm flows, either centered (`surface: 'dialog'`, the default) or anchored to a trigger (`surface: 'popover'`, requires `triggerEl`); a discriminated union on `surface` picks which options TypeScript actually offers you.
- [useDialogService](./useDialogService.md): The imperative engine behind `<Dialog>` for dialogs opened from code instead of markup.
- [usePopoverService](./usePopoverService.md): The imperative engine behind `<Popover>` for anchored popovers opened from code instead of markup.
- [useToast](./useToast.md): Sonner-style imperative toasts: call `toast(title, options)` from anywhere, no component context needed.
- [useTour](./useTour.md): The headless state machine behind `<Tour>` — index/group bookkeeping, step navigation, and the `onBeforeEnter`-await sequencing, with zero DOM or rendering baked in.

## Positioning & Data

- [useFloatingPosition](./useFloatingPosition.md): The Floating UI-backed positioning engine behind `Popover`, `Menu`, and `Tooltip`.
- [useVirtualizer](./useVirtualizer.md): Windowed rendering for long lists: only the visible rows (plus overscan) exist in the DOM.

## Utilities

- [useAsyncLoading](./useAsyncLoading.md): Tracks every in-flight promise passed to `run()`.
- [useColorScheme](./useColorScheme.md): Drives `document.documentElement.dataset.theme` from a `system` / `light` / `dark` mode.
- [useNumberFormat](./useNumberFormat.md): Locale-aware number formatting and parsing, both directions kept perfectly in sync.
- [useFieldControl](./useFieldControl.md): Wires a custom form control into the nearest `<Field>`: id/label association, `aria-describedby`/`aria-invalid`/`aria-required`, and reporting focus/filled state so Field can move a floating label or flip `data-filled`.

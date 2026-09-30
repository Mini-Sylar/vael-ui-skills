# DatePicker

A text field paired with a calendar popover for picking a date or range.

```ts
import { DatePicker } from 'vael-ui' // or 'vael-ui/vapor'
```

## Props

Name | Type | Default | Description
--- | --- | --- | ---
`selectionMode` | `CalendarSelectionMode \| undefined` | `'single'` | `'range'` picks a start then an end; clicking before the start restarts the range.
`view` | `CalendarView \| undefined` | `'date'` | What a click selects: a day, a month (its 1st) or a year (January 1st).
`placeholder` | `string \| undefined` |  | Text shown in the input when nothing is selected.
`disabled` | `boolean \| undefined` | `false` | Disables the input and keeps the panel from opening. A disabled parent Field also disables it.
`invalid` | `boolean \| undefined` | `false` | Standalone override; ORed with the nearest Field's `error` state.
`size` | `"md" \| "sm" \| "lg" \| undefined` | `'md'` | Input size.
`minDate` | `Date \| undefined` |  | Earliest selectable date (inclusive); the calendar disables earlier days, months and years.
`maxDate` | `Date \| undefined` |  | Latest selectable date (inclusive); the calendar disables later days, months and years.
`disabledDates` | `CalendarDisabledDates \| undefined` |  | List of unavailable dates, or a predicate function. Matching compares calendar days and ignores time.
`locale` | `string \| undefined` |  | BCP-47 locale for the input text, month and weekday names, and the week start day.
`firstDayOfWeek` | `number \| undefined` |  | `0` (Sunday) to `6` (Saturday). Unset, it derives from `locale` and falls back to Monday.
`formatOptions` | `Intl.DateTimeFormatOptions \| undefined` |  | `Intl.DateTimeFormat` options for the input text. Unset, the format follows `view`, `showTime` and `timeOnly`.
`name` | `string \| undefined` |  | Renders a hidden `<input>` that mirrors the selection as `YYYY-MM-DD` for form posts.
`side` | `Side \| undefined` | `'bottom'` | Which side of the trigger the panel opens on.
`align` | `Align \| undefined` | `'start'` | How the panel aligns against the trigger along that side.
`sideOffset` | `number \| undefined` | `8` | Gap between the trigger and the panel, in pixels.
`alignOffset` | `number \| undefined` | `0` | Shifts the panel along the alignment axis, in pixels.
`closeOnEsc` | `boolean \| undefined` | `true` | Escape key closes the panel.
`closeOnOutside` | `boolean \| undefined` | `true` | Clicking outside the panel closes it.
`beforeClose` | `((done: () => void) => void) \| undefined` |  | Custom exit animation; call `done()` to finish closing.
`forceMount` | `boolean \| undefined` | `false` | Keeps it mounted, toggled with `v-show`, so you can own the enter/exit animation.
`teleportTo` | `string \| HTMLElement \| undefined` | `'body'` | Teleport target: a CSS selector or element.
`motionCss` | `boolean \| undefined` | `true` | `false` skips both the popover enter/leave and Calendar's month-slide transitions.
`showButtonBar` | `boolean \| undefined` | `false` | Shows built-in Today (`single` mode only) and Clear buttons below the calendar. Has no effect once you provide the `#footer` slot, which renders instead.
`showTime` | `boolean \| undefined` | `false` | Adds an hour/minute row below the calendar (`single` mode only). Picking a date then keeps the panel open so you can set the time.
`timeOnly` | `boolean \| undefined` | `false` | Hides the calendar and shows only the time row. Implies `showTime`.
`hourFormat` | `"12" \| "24" \| undefined` |  | `'12'` adds an AM/PM toggle; `'24'` doesn't. Unset, it follows `locale` (or the runtime default) through `Intl`'s `hour12` resolution.
`minuteStep` | `number \| undefined` | `1` | Minute increment for the arrow-key and stepper-button adjustments.
`ui` | `Partial<{ root: UiPartValue; input: UiPartValue; positioner: UiPartValue; panel: UiPartValue; header: UiPartValue; navButton: UiPartValue; label: UiPartValue; weekdays: UiPartValue; weekday: UiPartValue; grid: UiPartValue; cell: UiPartValue; time: UiPartValue; footer: UiPartValue; }> \| undefined` |  | Class and style overrides for each part.
`modelValue` | `Date \| CalendarRange \| null \| undefined` | `null` | Selected date, or `{ start, end }` in `range` mode.
`open` | `boolean \| undefined` | `false` | Whether the panel is open.

## Slots

Name | Type | Description
--- | --- | ---
`footer` | `any` | Replaces the built-in Today/Clear button bar. It renders whenever you provide it, regardless of `showButtonBar`.

## Events

Name | Type | Description
--- | --- | ---
`open-change` | `[value: boolean, details: PopoverOpenChangeDetails]` | Fires when a close is requested; call `details.cancel()` to keep the panel open.
`update:open` | `[value: boolean]` | Fires when `open` changes (`v-model:open`).
`update:modelValue` | `[value: Date \| CalendarRange \| null]` | Fires when `modelValue` changes (`v-model`).
`change` | `[value: Date \| CalendarRange \| null]` | Fires when you pick a date, time or range endpoint, or use Today or Clear.
`month-change` | `[value: Date]` | Fires when the calendar's displayed month changes.

## Exposed

Name | Type | Description
--- | --- | ---
`el` | `HTMLElement \| null` | Root element.
`inputEl` | `HTMLInputElement \| null` | Native `<input>` element.
`panelEl` | `HTMLElement \| null` | Panel element (`null` while closed).
`positionerEl` | `HTMLElement \| null` | Positioning wrapper around the panel (`null` while closed).
`placement` | `Placement` | Resolved placement after flipping, e.g. `'bottom-start'`.
`positionerStyle` | `Record<string, string>` | Computed position styles applied to the positioner.
`isClosing` | `boolean` | `true` while a `beforeClose` close is pending.
`open` | `() => void` | Opens the panel unless disabled.
`close` | `() => void` | Closes the panel, running `@open-change` and `beforeClose` first.
`cancelClose` | `() => void` | Cancels a close pending in `beforeClose` and keeps the panel open. 


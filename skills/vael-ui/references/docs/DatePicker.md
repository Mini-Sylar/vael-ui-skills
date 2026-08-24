# DatePicker

A text field paired with a calendar popover for picking a date or range.

```ts
import { DatePicker } from 'vael-ui' // or 'vael-ui/vapor'
```

## Props

Name | Type | Default | Description
--- | --- | --- | ---
`selectionMode` | `CalendarSelectionMode \| undefined` | "single" | 
`view` | `CalendarView \| undefined` | "date" | 
`placeholder` | `string \| undefined` | undefined | 
`disabled` | `boolean \| undefined` | false | 
`invalid` | `boolean \| undefined` | false | 
`size` | `"md" \| "sm" \| "lg" \| undefined` | "md" | 
`minDate` | `Date \| undefined` | undefined | 
`maxDate` | `Date \| undefined` | undefined | 
`disabledDates` | `CalendarDisabledDates \| undefined` | undefined | 
`locale` | `string \| undefined` | undefined | 
`firstDayOfWeek` | `number \| undefined` | undefined | 
`formatOptions` | `Intl.DateTimeFormatOptions \| undefined` | undefined | Formats the trigger display. Default: `{ dateStyle: 'medium' }`.
`name` | `string \| undefined` | undefined | Hidden `<input>` mirroring selection as `YYYY-MM-DD` for form post.
`side` | `Side \| undefined` | "bottom" | 
`align` | `Align \| undefined` | "start" | 
`sideOffset` | `number \| undefined` | 8 | 
`alignOffset` | `number \| undefined` | 0 | 
`closeOnEsc` | `boolean \| undefined` | true | 
`closeOnOutside` | `boolean \| undefined` | true | 
`beforeClose` | `((done: () => void) => void) \| undefined` | undefined | 
`forceMount` | `boolean \| undefined` | false | 
`teleportTo` | `string \| HTMLElement \| undefined` | "body" | 
`motionCss` | `boolean \| undefined` | true | `false` skips popover enter/leave AND Calendar's month-slide transition.
`showButtonBar` | `boolean \| undefined` | false | Built-in Today (single mode only) / Clear buttons below the calendar. Ignored — the `#footer` slot always renders instead — once that slot is provided.
`showTime` | `boolean \| undefined` | false | Adds an hour/minute row below the calendar. `single` selection mode only — a range's two endpoints each having their own time isn't supported here. Keeps the popover open on a date pick instead of auto-closing, since there's still time left to set.
`timeOnly` | `boolean \| undefined` | false | Hides the calendar entirely — just the time row. Implies `showTime`.
`hourFormat` | `"12" \| "24" \| undefined` | undefined | `'12'` adds an AM/PM toggle; `'24'` doesn't. Default: resolved from `locale` (or the runtime default) via `Intl`'s own `hour12` resolution — an explicit value always wins.
`minuteStep` | `number \| undefined` | 1 | Minute increment for the arrow-key/stepper-button adjustments. Default: 1.
`ui` | `Partial<{ root: UiPartValue; input: UiPartValue; positioner: UiPartValue; panel: UiPartValue; header: UiPartValue; navButton: UiPartValue; label: UiPartValue; weekdays: UiPartValue; weekday: UiPartValue; grid: UiPartValue; cell: UiPartValue; time: UiPartValue; footer: UiPartValue; }> \| undefined` | undefined | 
`modelValue` | `Date \| CalendarRange \| null \| undefined` | null | 
`open` | `boolean \| undefined` | false | 

## Slots

Name | Type | Description
--- | --- | ---
`footer` | `any` | Replaces the built-in Today/Clear button bar entirely — shown whenever this slot is provided, regardless of `showButtonBar`.

## Events

Name | Type | Description
--- | --- | ---
`open-change` | `[value: boolean, details: PopoverOpenChangeDetails]` | 
`update:open` | `[value: boolean]` | 
`update:modelValue` | `[value: Date \| CalendarRange \| null]` | 
`change` | `[value: Date \| CalendarRange \| null]` | 
`month-change` | `[value: Date]` | 

## Exposed

Name | Type | Description
--- | --- | ---
`el` | `HTMLElement \| null` | 
`inputEl` | `HTMLInputElement \| null` | 
`panelEl` | `HTMLElement \| null` | 
`positionerEl` | `HTMLElement \| null` | 
`placement` | `Placement` | 
`positionerStyle` | `Record<string, string>` | 
`isClosing` | `boolean` | 
`close` | `() => void` | 
`cancelClose` | `() => void` | 


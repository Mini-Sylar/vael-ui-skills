# Component reference index

## Setup

- [ConfigProvider](./ConfigProvider.md): Wraps your app to provide theming, i18n, and class merging to every component inside it.

## Actions

- [Button](./Button.md): A clickable action with variants, sizes, loading states, and icon slots built in.
- [ButtonGroup](./ButtonGroup.md): Joins buttons into one connected row or column, and can turn them into single or multiple toggles.
- [SplitButton](./SplitButton.md): A primary action fused with a chevron that opens alternate actions in a dropdown.
- [Toolbar](./Toolbar.md): A horizontal strip for grouping related actions, with roving keyboard focus.
- [SpeedDial](./SpeedDial.md): A floating action button that fans out into secondary actions on demand.
- [Dial](./Dial.md): A circular drag control for picking a numeric value, like a volume knob.
- [SelectButton](./SelectButton.md): A segmented control for choosing one or more values from a short list.

## Forms & Inputs

- [Input](./Input.md): A single-line text field with sizes, states, and slots for icons or hints.
- [Textarea](./Textarea.md): A multi-line text field that can auto-grow with its content.
- [InputNumber](./InputNumber.md): A numeric field with increment and decrement controls and format-aware parsing.
- [OtpInput](./OtpInput.md): A row of single-character boxes for one-time codes, with paste and auto-advance.
- [PasswordInput](./PasswordInput.md): A password field with a reveal toggle and a requirements hint.
- [Editable](./Editable.md): Text that turns into an input when clicked and saves on Enter or blur, for renaming in place.
- [ColorPicker](./ColorPicker.md): A color swatch that opens a picker: a saturation and brightness plane, a hue slider, a hex field, an eyedropper where the browser has one, and optional presets.
- [Checkbox](./Checkbox.md): A checkbox for a single yes/no choice, with an optional indeterminate state.
- [Radio](./Radio.md): One option in a mutually exclusive set. Always use it inside a RadioGroup.
- [RadioGroup](./RadioGroup.md): Groups a set of Radio buttons so only one can be selected at a time.
- [Switch](./Switch.md): An on/off toggle styled like a physical switch, for immediate-effect settings.
- [Slider](./Slider.md): Drag a handle (or two, for a range) along a track to pick a numeric value.
- [Knob](./Knob.md): A rotary control for picking a numeric value within a fixed arc.
- [Field](./Field.md): Wraps a form control with a label, help text, and error message, wired together for you.
- [DatePicker](./DatePicker.md): A text field paired with a calendar popover for picking a date or range.
- [Calendar](./Calendar.md): A month grid for picking a single date or a date range.
- [FileUpload](./FileUpload.md): A drop zone and file list for picking files, with validation.
- [Rating](./Rating.md): Click, drag, or use arrow keys to pick a value on a row of stars, with optional half-step precision.

## Selection

- [Select](./Select.md): A dropdown for picking one or more values from a list, with filtering and virtualization.
- [Combobox](./Combobox.md): A searchable dropdown that also accepts free text, so autocomplete and tagging are one component.
- [CascadeSelect](./CascadeSelect.md): A dropdown for picking a path through nested options, one level at a time.
- [Tree](./Tree.md): A collapsible, indented list for hierarchical data, with single or multiple selection.
- [TreeSelect](./TreeSelect.md): A dropdown whose panel is a Tree, for picking one or more nodes from a hierarchy.

## Overlays

- [Dialog](./Dialog.md): A modal overlay that blocks the page until someone responds or dismisses it.
- [Drawer](./Drawer.md): A panel that slides in from a viewport edge, for navigation or side content.
- [Popover](./Popover.md): A floating panel anchored to a trigger element, for menus, forms, or extra detail.
- [Tooltip](./Tooltip.md): A small floating label that appears on hover or focus to explain an element.
- [BottomSheet](./BottomSheet.md): A panel that slides up from the bottom edge, for touch layouts.
- [ContextMenu](./ContextMenu.md): A menu that opens at the pointer on right-click, replacing the native browser menu.
- [CommandPalette](./CommandPalette.md): A searchable list of commands in a dialog, opened from a button or a keyboard shortcut such as ⌘K.
- [Tour](./Tour.md): A guided walkthrough that highlights one element at a time next to a callout, for onboarding and product tours.

## Navigation & Menus

- [Menu](./Menu.md): A dropdown list of actions opened from a trigger, with full keyboard navigation.
- [MenuList](./MenuList.md): Menu's rows rendered in the page flow instead of a dropdown, for a permanent sidebar nav.
- [Tabs](./Tabs.md): Switches between panels of content, with a sliding indicator under the active tab.
- [Stepper](./Stepper.md): Shows progress through a sequence of steps, and lets people move between them.
- [Breadcrumb](./Breadcrumb.md): A trail of links from the top of the site down to the current page.

## Feedback

- [Toaster](./Toaster.md): Renders the notifications that toast() creates. Mount it once, anywhere in your app.
- [Message](./Message.md): An inline banner for status text: info, success, warning, or error.
- [Progress](./Progress.md): A progress bar for a task or stacked shares of a whole, with determinate and indeterminate states and an optional legend.
- [Loader](./Loader.md): A small spinner that shows something is in progress.
- [Skeleton](./Skeleton.md): A placeholder shape that stands in for content while it loads.

## Data Display

- [DataTable](./DataTable.md): A data grid with sorting, selection, column resizing, and virtualization, built from Column components.
- [Pagination](./Pagination.md): Page-number controls for splitting a long list or table across pages.
- [Card](./Card.md): A bordered container for grouping related content, with optional title and actions.
- [Avatar](./Avatar.md): A circular or square image or initials that represents a person or entity.
- [AvatarGroup](./AvatarGroup.md): Stacks several avatars into one overlapping row, with a count for the ones that don’t fit.
- [Badge](./Badge.md): A small dot, count, or label overlaid on the corner of another element.
- [Tag](./Tag.md): A compact label for categorizing or annotating an item inline.
- [Chip](./Chip.md): A removable, compact token for selected filters, tags, or multi-select values.
- [Kbd](./Kbd.md): Styles a keyboard shortcut so it reads like a physical key.
- [AnimatedNumber](./AnimatedNumber.md): Shows a number that rolls each changed digit to its new value, formatted for any locale.
- [AnimatedText](./AnimatedText.md): Swaps one text for the next with a short fade and lift, easing the width so nothing jumps.
- [Timeline](./Timeline.md): An ordered list of steps joined by a line. Each step can hold any content.

## Layout & Structure

- [Accordion](./Accordion.md): A stack of collapsible sections where opening one can close the others.
- [Collapsible](./Collapsible.md): A single show/hide section for optional or secondary content.
- [Separator](./Separator.md): A thin line, with an optional label, that divides content into groups.
- [Resizable](./Resizable.md): A drag handle that resizes a panel between a minimum and maximum size.
- [ScrollArea](./ScrollArea.md): A scroll box with slim, themed scrollbars and faded edges, in one or both directions.

## Gestures

- [SwipeToReveal](./SwipeToReveal.md): Swipe an item to reveal actions underneath it, like a native mobile list row.
- [PullToRefresh](./PullToRefresh.md): A pull-down gesture at the top of a scroll container that triggers a refresh.
- [Dock](./Dock.md): A macOS-style row of icons that magnify as the pointer approaches them.
- [Sortable](./Sortable.md): A drag-to-reorder list that also works from the keyboard, not only with a pointer.

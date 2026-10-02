# Styling and cascade layers

vael-ui ships every component's CSS inside `@layer ui-components`. **Unlayered CSS always wins over a layered rule regardless of selector specificity**, so a plain, unlayered base reset (a lot of resets/normalize.css are unlayered by default) silently overrides vael-ui internals no matter how unrelated it looks. Fix: put the app's own base styles in a layer too, ordered *before* `ui-components`. Layer order is decided by order of first appearance in the document, not by CSS source order or specificity.

```css
/* Import this before any component CSS loads, to pin the order explicitly */
@layer app-base, ui-components;
```

Deliberately overriding one component (a `ui` prop class, a Tailwind utility) wants the opposite: keep that override CSS **unlayered**, which it already is by default in a normal `<style>` block or utility class. That's what lets it win over the layered internals on purpose.

Everything before `0.2.0` didn't split CSS per component. Only relevant on that old a version, or if you specifically want everything loaded at once regardless of what's used.

## CSS variables

Every color, radius, shadow, and motion value reads from a CSS custom property. Override any of them in an own unlayered stylesheet; no build step required. `ConfigProvider`'s `theme` prop covers `primary`/`danger`/`radius` for dynamic, JS-driven theming; everything below is CSS-only.

### Colors

| Variable | Default (light) | Description |
| --- | --- | --- |
| `--ui-primary` | `#18181b` | Primary action color (buttons, links, focus accents). |
| `--ui-primary-hover` | `#27272a` | Hover state for the primary color. |
| `--ui-primary-contrast` | `#fafafa` | Text/icon color on top of the primary color. |
| `--ui-muted` | `#f4f4f5` | Subtle background for secondary surfaces. |
| `--ui-muted-hover` | `#e4e4e7` | Hover state for muted backgrounds. |
| `--ui-danger` | `oklch(0.586 0.253 26)` | Destructive/error color. |
| `--ui-success` | `oklch(71.335% 0.15901 160.899)` | Success color. |
| `--ui-warning` | `oklch(0.666 0.179 58.315)` | Warning color. |
| `--ui-info` | `#2563eb` | Informational color. |
| `--ui-surface` | `#ffffff` | Base background for panels, cards, and dialogs. |
| `--ui-text` | `#18181b` | Primary text color. |
| `--ui-text-muted` | `#71717a` | Secondary/muted text color. |
| `--ui-border` | `#e4e4e7` | Default border color. |
| `--ui-border-strong` | `#d4d4d8` | Stronger border color for emphasized edges. |
| `--ui-overlay` | `rgb(0 0 0 / 0.4)` | Backdrop color behind modals and dialogs. |

Each semantic color (`danger`/`success`/`warning`/`info`) also has its own `-hover` and `-contrast` variants, same pattern as `primary` above. Dark mode redefines the same variable names under `:root:not([data-theme="light"])`/`[data-theme="dark"]`; it never introduces new ones.

### Shape

| Variable | Default | Description |
| --- | --- | --- |
| `--ui-radius` | `10px` | Base corner radius for controls (buttons, inputs, badges). |
| `--ui-radius-surface` | `min(var(--ui-radius), 1rem)` | Capped radius for large surfaces (panels, cards, tables), so an aggressive `--ui-radius` can't turn them into a stadium shape. |

### Elevation & layering

| Variable | Default | Description |
| --- | --- | --- |
| `--ui-z-dialog` | `50` | Stacking order for dialogs. |
| `--ui-z-tour` | `52` | Stacking order for Tour's spotlight overlay. |
| `--ui-z-popover` | `55` | Stacking order for popovers, menus, and selects. |
| `--ui-z-toast` | `60` | Stacking order for toasts. |
| `--ui-z-tooltip` | `70` | Stacking order for tooltips (topmost). |
| `--ui-panel-shadow` | _(multi-value)_ | Drop shadow for floating panels. |

### Motion

| Variable | Default | Description |
| --- | --- | --- |
| `--ui-ease-out` | `cubic-bezier(0.23, 1, 0.32, 1)` | Default easing for entering/settling transitions. |
| `--ui-ease-in-out` | `cubic-bezier(0.77, 0, 0.175, 1)` | Easing for on-screen movement (resize, reposition). |
| `--ui-ease-drawer` | `cubic-bezier(0.32, 0.72, 0, 1)` | Easing for edge-anchored slides (drawers, sheets). |
| `--ui-duration-drawer` | `500ms` | Duration for drawer/sheet slide transitions. |
| `--ui-duration-press` | `160ms` | Duration for press/tap feedback. |
| `--ui-duration-enter` | `200ms` | Duration for enter/open transitions. |
| `--ui-duration-exit` | `150ms` | Duration for exit/close transitions. |
| `--ui-duration-tooltip` | `125ms` | Duration for tooltip open. |
| `--ui-duration-tooltip-exit` | `100ms` | Duration for tooltip close. |
| `--ui-duration-toast` | `400ms` | Duration for toast enter. |
| `--ui-duration-toast-exit` | `200ms` | Duration for toast exit. |
| `--ui-ease-toast` | `ease` | Easing for toast transitions. |
| `--ui-toast-offset` | `1rem` | Edge offset for the toast stack. |

## Advanced: joining your own control to a Field

Not needed for everyday use; only for custom controls that should plug into a Field's joined cells.

A Field's `attached` label and `#prepend`/`#append` cells join the control's bordered frame. vael-ui's own controls mark that frame with `data-ui-frame`. Add the same attribute to your own control and it joins too. Give it the control's size (`sm`, `md` or `lg`) and the cells match its corner radius and font size.

```vue
<Field label="Handle">
  <template #prepend>@</template>
  <input class="my-input" data-ui-frame="md" />
</Field>
```

Field's join rules are in `@layer ui-components`, so if your control's own styles are unlayered, they win. In that case, flatten the corners and recolor the border yourself with the attributes Field sets on its root:

```css
/* unlayered, so Field's own join rules can't reach .my-input */
.ui-field[data-join-start] .my-input {
  border-start-start-radius: 0;
  border-end-start-radius: 0;
}
.ui-field[data-join-end] .my-input {
  border-start-end-radius: 0;
  border-end-end-radius: 0;
}
.ui-field[data-focused] .my-input {
  border-color: var(--ui-primary);
}
```

Give a control inside a cell its own `aria-label`. It stands alone, so the Field's label doesn't name it.

### Field attributes (advanced)

| Attribute | Set when |
| --- | --- |
| `data-ui-frame` | On your control's bordered element, not on Field. Optional value: sm, md or lg. |
| `data-join-start` | A cell or an attached label sits on the control's start edge. |
| `data-join-end` | A cell or an attached label sits on the control's end edge. |
| `data-focused` | The control has focus. |
| `data-invalid` | The Field has an error. |
| `data-attached` | Field draws the label as a joined cell. |

### Field variables

| Variable | Default | Description |
| --- | --- | --- |
| `--ui-field-cell-bg` | `var(--ui-muted)` | Background of joined cells: an attached label, #prepend and #append. |
| `--ui-field-cell-color` | `var(--ui-text-muted)` | Text color of joined cells. |
| `--ui-field-label-width` | `max-content` | Width of a start/end label. Set it on a parent to line up a whole form. |

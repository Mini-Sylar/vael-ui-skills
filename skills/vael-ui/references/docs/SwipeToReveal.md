# SwipeToReveal

Swipe an item to reveal actions underneath it, like a native mobile list row.

```ts
import { SwipeToReveal } from 'vael-ui' // or 'vael-ui/vapor'
```

## Props

Name | Type | Default | Description
--- | --- | --- | ---
`side` | `SwipeRevealSide \| undefined` | "trailing" | 
`disabled` | `boolean \| undefined` | false | 
`ui` | `Partial<{ root: UiPartValue; content: UiPartValue; actions: UiPartValue; }> \| undefined` | undefined | 
`open` | `boolean \| undefined` | false | 

## Slots

Name | Type | Description
--- | --- | ---
`default` | `{ open: boolean; reveal: () => void; close: () => void; }` | 
`actions` | `{ open: boolean; close: () => void; }` | 

## Events

Name | Type | Description
--- | --- | ---
`update:open` | `[value: boolean]` | 
`change` | `[open: boolean]` | 

## Exposed

Name | Type | Description
--- | --- | ---
`el` | `HTMLElement \| null` | 
`contentEl` | `HTMLElement \| null` | 
`actionsEl` | `HTMLElement \| null` | 
`isDragging` | `boolean` | 
`reveal` | `() => void` | 
`close` | `() => void` | 


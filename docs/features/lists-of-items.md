# Lists of items

## ListLocator

The `ListLocator` is designed to display a list of items:

```typescript jsx
export class RiverLocator extends ListLocator {
  coordinates = { x: 20, y: 10 }
  gap = { x: 6 }
}
```
- By default, items are indexed by their x coordinates if defined (a fallback to y and z coordinates exists). You can override `getItemIndex` to change the default order.
- The coordinates will match the center of the item with index 0.
- The gap will be the difference between 2 successive items. If the gap is on y, the list will be displayed in a column. Using x AND y can display the list in diagonal.
- You can use `maxCount` if you want the list to stop expanding after a number of items:

```typescript jsx

export class RiverLocator extends ListLocator {
  coordinates = { x: 20, y: 10 }
  gap = { x: 6 }
  maxCount = 5 // if there are more than 5 items, the gap will close by so that the list does not take more space.
}
```

- You can use `maxGap` instead of `maxCount` if the maximum space taken by the list is not a multiple of the gap.
- All properties can be dynamic, using `getGap`, `getMaxCount`, `getMaxGap`, etc.

## FlexLocator

The FlexLocator is a subclass of the list locator that allows items that only have a single index in the data (only a x-coordinate for example) to be displayed on multiple lines on in the app.

It is inspired by the [CSS flexbox](https://css-tricks.com/snippets/css/a-guide-to-flexbox/), but it is much simpler and with fewer features.

```typescript jsx
export class MultilineRiveLocator extends FlexLocator {
  coordinates = { x: 20, y: 10 }
  gap = { x: 6 }
  lineSize = 3
  lineGap = { y: 10 }
  maxLines = 4
}
```

- `lineSize` defines how many items will fit in a "line" before a new "line" is created
- `lineGap` is the gap between 2 "lines".
- :bulb: lines can be columns and columns can be line if the gap is on y, and the lineGap on x.
- `maxLines` is the number of lines before the lines closes by.
- `maxLineGap` can be used instead of `maxLines` if the maximum number of lines is not a multiple of the `lineGap`.
- All properties can be dynamic, using `getLineSize`, `getLineGap`, `getMaxLines`, etc.

## DeckLocator

The `DeckLocator` is a default case of list locator with a maximum number of 20 items displayed **starting from the top** and a default gap: `{ x: -0.05, y: -0.05 }`.

You can change the maximum number of items displayed using the `limit` property from the Locator.

# Piles of items

You can use a `PileLocator` to display the items as a disorganized pile:

```typescript jsx
class ResourcePileLocator extends PileLocator {
  coordinates = { x: 20, y: 10 }
  limit = 20 // default value
  radius = 0 // default value
  maxAngle = 180 // default value
}
```

- The `radius` is the maximum dispersion the items can have around the `coordinates` (in centimeters)
- The `radius` can be 2 coordinates for an elliptic dispersion: `radius = { x: 2, y: 1 }`
- The `limit` is the maximum number of items displayed
- The `maxAngle` is the maximum rotation the items can have **in both way**. 180 means the items can go from -180° to 180°, so a full rotation.
- The PileLocator will process random coordinates and rotations to each new items and remember it as long as the page is not refreshed. If the player refreshes the page, the items will get different positions.
- All the properties can be dynamic, using `getRadius`, `getMaxAngle`, etc.

## Multiple piles

When you want to display multiple piles with the same locator, each pile has an identifier.

By default, each pile is identified using the `location.player`, `location.id` or `location.parent` fields:

```
getPileId(item: MaterialItem<P, L>, _context: ItemContext<P, M, L>): string {
    return [item.location.player, item.location.id, item.location.parent].filter(part => part !== undefined).join('_')
}
```

You can override `getPileId` to create different piles based on something else, like the item id.

If you want to create a stock for coins of different values for example, you need the same location for the [Money utility](features/items-with-quantity.md#money) to work, but you need different piles for the coins to display properly:

```
getPileId(item: MaterialItem) {
    return item.id
}
```


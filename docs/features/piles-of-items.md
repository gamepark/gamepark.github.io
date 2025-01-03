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

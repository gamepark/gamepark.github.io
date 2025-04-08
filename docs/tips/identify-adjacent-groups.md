# Identify adjacent groups

In some game, the scoring involves to count adjacent items on a map. For example:

- in Solstis, you score the largest group of adjacent tiles
- in Kingdomino, you score each group of adjacent terrain x the number of crown in the group
- in Hanging Gardens (Les Jardins Suspendus), you score the largest connected group in each flower color

The grouping algorithm is a bit tricky to write, so we create a utility function in the rules-api: `createAdjacentGroups`

Example:

```typescript jsx
const map = [
  [1, 2, 0, 1]
  [0, 1, 3, 1]
  [1, 0, 0, 0]
]
const groups = createAdjacentGroups(map)
// you get:
const group1 = { values: [1, 2, 1, 1, 3, 1], coordinates: [/* all the XYcoordinates of this group */] }
const group2 = { values: [1], coordinates: [{ x: 0, y: 2 }] }
/*
groups = [
  [group1, group1, empty, group1]
  [empty, group1, group1, group1]
  [group2, empty, empty, empty]
]
*/
```

You can pass any type of data in the map: use the `options.isEmpty` parameter to identify empty locations: `createAdjacentGroups(map, {isEmpty: ...})`

You can map groups in a hexagonal grid (only axial coordinates are implement right now): `createAdjacentGroups(map, { hexGridSystem: HexGridSystem.Axial })`

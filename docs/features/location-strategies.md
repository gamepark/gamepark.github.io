# Location strategies

A location strategy is a class that will be called everytime an item is added, move inside, or remove from a [location area](concepts/location-area.md), and that can update all the items sharing the same location area.

Everytime you need to maintain some consistent items locations between items that share the same location area, you should use a location strategy.

Location strategies are defined on the top rules class, material type by material type, and location type by location type:

```typescript
export class GameTemplateRules {
  locationsStrategies = {
    [MaterialType.Card]: {
      [LocationType.Deck]: new PositiveSequenceStrategy(),
      [LocationType.PlayerHand]: new PositiveSequenceStrategy(),
      [LocationType.River]: new FillGapStrategy()
    }
  }
}
```

The framework offers a few location strategies that are very common across games. For more specific use cases, you can implement a custom location strategy.

## Positive sequence strategy

Use that strategy everytime you need to have inside a location area items going from x=0 to x=n-1, without a gap.

This strategy is used for piles or hands of cards for examples.

If an item **with no X** is added to the list, the item will take X=number of items before it was added.

If an item is added **with a X**, all the existing items with the same X or higher will increment their X by 1.

:bulb: if you want to put a card **under a deck**, simply move it with x=0.

If an item is removed from the list, all items with a superior X will decrement their X by 1.

Finally, you can also move an item inside the list: if you move an item to the same location area, with or without an X.

In the case, all the items between the new item X (which is the last position if X was not defined) and the previous X will increment or decrement their X by 1.

## Fill gap strategy

Using that strategy, when an item is removed from the list, it will leave a gap inside the sequence.

If an item is added, it will fill in the first gap, or go to the end of the list is there were no gaps.

Use it for rivers of cards for example.

## Stacking strategy

Usage: `new StackingStrategy()`

A stacking strategy will maintain a consistent sequence of `location.z` for all the items that share the same X and Y.

It works the same way as the Position sequence strategy, but working on Z and only caring about items with the same X and Y.

Use it to stack items that sometimes shares the same position on a board: for example, score pawns that you put on top of each others when they share the same space.

## Custom location strategies

Here is the signature of the location strategy:
```typescript
export type LocationStrategy = {
  /**
   * Strategy to apply when an item is added to a location area
   * @param material The material that exists in the same location area before the new item is added
   * @param item The item that is going to be added
   */
  addItem?(material: Material, item: MaterialItem): void

  /**
   * Strategy to apply when an item is moved inside a location area (only x, y, z or rotation changes)
   * @param material The material that exists in the same location area
   * @param item The item that is going to move inside the location area
   * @param index Index of the moved item in the Material
   */
  moveItem?(material: Material, item: MaterialItem, index: number): void

  /**
   * Strategy to apply when an item is moved inside a location area (only x, y, z or rotation changes)
   * @param material The material that remain in the same location area after the item was removed
   * @param item The item that was just removed from the location area, with the state it had before it was removed
   */
  removeItem?(material: Material, item: MaterialItem): void
}
```

You can implement this type when you have more complex rules about how to place items in a game.

Example: the [Civilisation Area in Along History](https://github.com/gamepark/along-history/blob/main/rules/src/util/CivilisationAreaStrategy.ts)

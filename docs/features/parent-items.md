# Parent items

When an item is placed on top of another one, we call it its "parent item".

Using the parent item feature allows to place items relatively to their parent item, and to drag and drop multiple items at once.

## Static parent item

Placing an item on a static board for example is very common. [Static items](step-by-step-example/display-first-item?id=display-a-static-item) are items that does not need to be in the game state because they never move.

All you need to do to place an item relative to a **unique** static item is to specify the parent item type in the locator, as in [this example from Architects of Amytis](https://github.com/gamepark/architects-of-amytis/blob/main/app/src/locators/MainBoardStackSpaceLocator.ts):

```typescript
class MainBoardStackSpaceLocator extends Locator {
  parentItemType = MaterialType.MainBoard
  
  //...
}
```

The static parent item is automatically inferred from the staticItem property in the [Material Description](https://github.com/gamepark/architects-of-amytis/blob/main/app/src/material/MainBoardDescription.ts):
```typescript
class MainBoardDescription extends BoardDescription {
  staticItem = { location: { type: LocationType.MainBoardSpot } }
}
```

However, if there are multiple static items of this type, and you implemented `staticItems` or `getStaticItems` in the material description, then you must specify the parent item in the Locator, as in [this example from Architects of Amytis](https://github.com/gamepark/architects-of-amytis/blob/main/app/src/locators/PlayerBoardStackSpaceLocator.ts):
```typescript
class PlayerBoardStackSpaceLocator extends Locator {
  parentItemType = MaterialType.PlayerBoard

  getParentItem(location: Location) {
    return playerBoardDescription.staticItems.find(item => item.location.player === location.player)
  }
}
```

## Dynamic parent item

If the parent item can move, it is an item inside the game state.

In that case, you must use the `location.parent` property to refer to the parent item's index:

```typescript
const cardIndex = this.material(MaterialType.Card).location(LocationType.Somewhere)/*other filters...*/.getIndex()
this.createItem(MaterialType.Token, { location: { type: LocationType.OnCard, parent: cardIndex } })
```

Inside the locator, you only need to specify the parent item type:
```typescript
class OnCardLocator extends Locator {
  parentItemType = MaterialType.Card
}
```

The item will automatically be placed relative to the parent card based on the `location.parent` property.

## Position on parent

Once the parent item is known inside the locator, you can place the item relative to the parent item using either `coordinates` (in centimeters, as usual) or `positionOnParent` (or both).

The position on parent allows to place an item **as a percentage or the width and height of the parent**:

```typescript
class OnCardLocator extends Locator {
  parentItemType = MaterialType.Card
  
  positionOnParent = { x: 40, y: 20 } // 40% from the left and 20% from the top
}
```

By default, it is centered. For a dynamic position on parent use `getPositionOnParent`.

## Parent rotations

When an item is placed on a parent, it will automatically inherit all the parent CSS transformations of parent item's own locator.

This includes rotations of course. While this is interesting for z-rotations, it can be a problem for x or y rotations.

For this reason, cards are usually flipped using the Material Description (`isFlipped`) instead of the locators.

## Multiple parents

An item can only have one parent, however its parent can have a parent itself.

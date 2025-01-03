# Place items

## The Locators

The static and dynamic items created are displayed by default in the middle of the "Table".

The Table is a coordinates system, in centimeters, that we use to position the items on the screen.

To change the position of an item, we need to add a locator for its location type in the [Locators file](https://github.com/gamepark/board-game-template/blob/main/app/src/locators/Locators.ts):

```typescript
import { Locator } from '@gamepark/react-game'

class MainBoardSpotLocator extends Locator {
  coordinates = { x: 10, y: 10 }
}

export const mainBoardSpotLocator = new MainBoardSpotLocator()
```

This locator simply moves the items 10 centimeters to the left and 10 centimeters to the bottom.

## Dynamic position

You can place an item based on the location properties and the display context:

```typescript
class MainBoardSpotLocator extends Locator {
  getCoordinates(location: Location, context: MaterialContext) {
    if (context.player === location.player) {
      return { x: location.x! * 5 }
    } else {
      return { x: location.x! * 5, y: -20 }
    }
  }
}
```

`context.player` is the player displaying the game. :warning: For spectators, it is undefined!

`location.x` is an optional field on the Location object. [More about Location objects](concepts/items-and-locations.md#location-properties).

Depending on what you want to do, we have a lot of Locators feature built in the framework:
* You can [display items on other items](features/parent-items.md)
* Use the [ListLocator](features/lists-of-items.md#listlocator) to display a list of items
* Use the [HandLocator](features/hand-of-cards.md) to display items in a fan shape
* Use the [FlexLocator](features/lists-of-items.md#flexlocator) inspired from the CSS flexbox
* Use the [DeckLocator](features/lists-of-items.md#decklocator) for deck of cards
* Use the [PileLocator](features/piles-of-items.md) for disorganized piles of item

## Location strategies

Many locators depend on the `location.x` attribute. Usually, a locator expects items to form a sequence from x=0 to x=n-1 without any gap.

Maintaining a proper sequence for `location.x` on every item of a list when they move can be complex.

That's why we created the "location strategies":
```typescript
export class GameTemplateRules {
  locationStrategies = {
    [MaterialType.Card]: {
      [LocationType.Deck]: new PositiveSequenceStrategy(),
      [LocationType.PlayerHand]: new PositiveSequenceStrategy(),
      [LocationType.River]: new FillGapStrategy()
    }
  }
}
```

The responsibility for a location strategy is to maintain the consistency of the properties `x`, `y`, `z` of items sharing the same [**location area**](concepts/location-area.md). Items share the same location area when they have the same location `type`, `id`, `player` and `parent`:
* `location.type` is the mandatory LocationType
* `location.player` is the optional owner player
* `location.parent` is the optional parent item (example: a token on a card)
* `location.id` is a free-to-use field to differentiate locations

The `PositiveSequenceStrategy` will make sure item starts with x = 0 to x = n-1 everytime an item move in, inside or out the location area:
* If you add an item without specify a `location.x`, then it will take the highest x.
* If you specify `location.x`, every item with the same or higher x will move by x+1. 
* If an item is removed, every item with a higher x will move by x-1 automatically.

The `FillGapStrategy` is similar to the `PositiveSequenceStrategy`, except that when an item is remove, the gap is not filled, until another item is added.

:confetti_ball: With the location strategies, no need to take care of x, y and z anymore when moving most items! You can even create custom location strategies for specific game rules.

[Learn move about location strategies](features/location-strategies.md)

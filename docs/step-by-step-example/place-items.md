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

This locator simply moves the items 10 centimeters to the right and 10 centimeters to the bottom.

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
  locationsStrategies = {
    [MaterialType.Card]: {
      [LocationType.Deck]: new PositiveSequenceStrategy(),
      [LocationType.PlayerHand]: new PositiveSequenceStrategy(),
      [LocationType.River]: new FillGapStrategy()
    }
  }
}
```

A location strategy maintains the `x`, `y` and `z` of the items sharing the same [**location area**](concepts/location-area.md).

:confetti_ball: With the location strategies, no need to take care of x, y and z anymore when moving most items! You can even create custom location strategies for specific game rules.

[Learn more about location strategies](features/location-strategies.md)

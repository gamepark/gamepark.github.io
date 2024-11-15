# Place items

## The Locators

The static and dynamic items created are displayed by default in the middle of the "Table".

The Table is a coordinates system, in centimeters, that we use to position the items on the screen.

To change the position of an item, we need to add a locator for its location type in the [Locators file](https://github.com/gamepark/colt-super-express/blob/main/app/src/locators/Locators.tsx):

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

`location.x` is an optional field on the Location object. [More about Location objects](TODO).

Depending on what you want to do, we have a lot of Locators feature built in the framework:
- You can [display items on other items](TODO)
- Use the [ListLocator](TODO) to display a list of items
- Use the [HandLocator](TODO) to display items in a fan shape
- Use the [FlexLocator](TODO) inspired from the CSS flexbox
- Use the [DeckLocator](TODO) for deck of cards

# Display a first item

## The Material Description

The description for each type of material must be listed in the [Material file](https://github.com/gamepark/board-game-template/blob/main/app/src/material/Material.ts)

Create a new file to describe your item. Here is a simple example:

```typescript
import { BoardDescription } from '@gamepark/react-game'
import MainBoard from '../images/boards/MainBoard.jpg'

class MainBoardDescription extends BoardDescription {
  width = 20
  height = 20
  image = MainBoard
}

export const mainBoardDescription = new MainBoardDescription()
```

By convention, the file and the class are named after the MaterialType, ending with "Description".

The width and the height must be expressed in centimeters.

We export a singleton instance of the class.

:bulb: you can extend `BoardDescription`, `CardDescription`, `TokenDescription` or `CubicDiceDescription` from the framework

## Display a Static item

The Material description is used to display **Material items**.

Without any item to display, nothing will appear on the screen. We need to create a first item.

Some items will never move during a game (boards usually don't move for example). We call them "Static items".

Inside the BoardDescription class, you can add a static item this way:
```typescript
staticItem = { location: { type: LocationType.MainBoardSpot } }
```

The minimum required information on an item, is a location. And the minimum required information on a location is its type.

Now, the image of your board will be displayed on the screen.

In the [next step](step-by-step-example/create-items.md), you will see how to create and display dynamic items.

:bulb: You could use only dynamic items to show any item in the game. However, it would use more space in the database, more bandwidth and more memory to display.

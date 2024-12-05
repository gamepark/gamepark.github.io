# Items ID

The item ID is used everytime different items of the same type have different images.

## Enumerate the IDs

The best practice is usually to create an enum, for example:

```typescript
export enum CardColor {
  Blue = 1, Red, Green, Yellow
}
export const cardColors = getEnumValues(CardColor)
```
`getEnumValues` utility create an array with the enum values (here: `[1, 2, 3, 4]`).

## Create items with ID

Each item has an optional `id` property:

```typescript
export class ExampleGameSetup extends MaterialGameSetup {
  setupMaterial() {
    this.material(MaterialType.SomeCard).createItems(cardColors.map(cardColor => ({ id: cardColor, location: { type: LocationType.SomeSpace } })))
  }
}
```
In this example, on card of each color is created in the game state:
```
{
  items: {
    1: [
      {id: 1, location: {type: 2}},
      {id: 2, location: {type: 2}},
      {id: 3, location: {type: 2}},
      {id: 4, location: {type: 2}},
    ]
  }
  players: [1, 2]
  ...
}
```

:warning: The item id is not a unique identifier for the item: if you have 2 cards that look exactly the same in the game, they have the same id. We use the item's index (in the game state) to identify it uniquely.

For more complex use cases, have a look at:
- [Hiding only one side of a card](features/cards-with-different-backs.md)
- [Pro tips for composite IDs](TODO)

## Images based on ids

Now, inside the MaterialDescription in the app, you can map each id with its image:

```typescript
import { CardDescription } from '@gamepark/react-game'
import BlueCard from '../images/cards/BlueCard.jpg'
import RedCard from '../images/cards/RedCard.jpg'
import GreenCard from '../images/cards/GreenCard.jpg'
import YellowCard from '../images/cards/YellowCard.jpg'

class SomeCardDescription extends CardDescription {
  images = {
    [CardColor.Blue]: BlueCard,
    [CardColor.Red]: RedCard,
    [CardColor.Green]: GreenCard,
    [CardColor.Yellow]: YellowCard,
  }
}
```
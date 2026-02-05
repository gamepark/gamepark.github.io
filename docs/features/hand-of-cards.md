# Hand of cards

Having cards in their hands is a very common feature in games.

## Cards locations

For cards in hands, you items should have that location:
```typescript jsx
{
  location: {
    type: LocationType.PlayerHand // for example
    player: player // player holding the cards
    x: 0 // 1, 2, 3
  }
}
```

You should use the [PositiveSequenceStrategy](features/location-strategies.md#positive-sequence-strategy) for this location.

## Secure the data

As the hand should only be seen by the player holding it, the game should extend `SecretMaterialRules` and the `hideItemIdToOthers` strategy should be used. [More info here](step-by-step-example/hide-the-cards.md)

If [cards have different backs](features/cards-with-different-backs.md), you will use `hideFrontToOthers` instead.

## HandLocator

The `HandLocator` available in the framework allows you to display a hand of items in the shape of a fan:

```typescript jsx
class PlayerHandLocator extends HandLocator {
  coordinates = { x: 20, y: 10 }
  radius = 100 // default value
  baseAngle = 0 // default value
  maxAngle = 15 // default value
  gapMaxAngle = 3 // default value
  clockwise = true // default value
}
```

- if there is an odd number of cards in the hand, the `coordinates` will match the center of the card in the middle of the list.
- the `radius` is the distance in centimeter between the center of the cards and the origin of the circle they rotate around. Increasing the radius will make the fan more flattish.
- the `baseAngle` is the rotation of the hand in degrees. 180 will display the hand upside down.
- the `maxAngle` is the maximum angle between the first and the last card in the hand before cards close-by.
- the `gapMaxAngle` is the maximum angle between 2 cards. If there are only 2 cards in the hand, they cannot have more space between them.
- set `clockwise` to false if the fan should open from the left to the right.

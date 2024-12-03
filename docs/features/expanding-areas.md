# Expanding areas

In some game, you place tiles or cards on the table, and this areas can grow in any direction.

Here is some tips to handle that will a good UI.

## Tableau of cards

In [Château Combo](https://github.com/gamepark/chateau-combo/blob/main/app/src/locators/TableauLocator.ts) of [Living Forest Duel](https://github.com/gamepark/living-forest-duel/blob/main/app/src/locators/PlayerForestLocator.ts) for example, players build a tableau of cards. The size is limited to 3x3, but it can grow in any direction.

You can easily center the tableau in the area based on the boundaries of the cards in the game state:

```typescript jsx
const { xMax, xMin, yMax, yMin } = new TableauHelper(context.rules.game, location.player!).boundaries
const { x, y } = this.getBaseCoordinates(location, context)
const deltaX = (xMin + xMax) / 2
const deltaY = (yMin + yMax) / 2
return {
  x: x + (location.x! - deltaX) * (cardDescription.width + 0.2),
  y: y + (location.y! - deltaY) * (cardDescription.height + 0.2)
}
```

This way, if the maximum number of cards is 3, you only need enough space for 4 cards for example: 2 cards + 2 drop areas on each side.

## Infinite areas

For infinite areas like in Rivality or Spring Festival we try to follow a few rules for a good user experience:

- make the table size dynamic so that is grows when necessary.
- reserve a minimum space for the default zone, so that the table does not change size all the time.
- you can place the players items around the area and moves those items dynamically depending on the size required for the area.
- cache the delta applied to the area items in the locator, and only center the items when necessary, to limit the number of time the items are slided.

This kind of item placement is hard to implement with a great UI, so do not hesitate to ask for help!

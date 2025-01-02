# Predictable cards

If players take in their hand a card which is visible to other players, the card can be tracked. Therefore, if you have to play a card face down from your hand afterward, the card played can be predicted by other players.

To prevent this issue, you have to shuffle the player's hand after picking the card, like in [Faraway](https://github.com/gamepark/faraway/blob/main/rules/src/rules/ChooseNewRegionCardRule.ts#L20).

You may still want to display the cards in a non-random order on the client side. You can do that by [overriding `getItemIndex` in the locator](https://github.com/gamepark/faraway/blob/main/app/src/locators/RegionHandLocator.ts#L44):

```typescript jsx
class RegionHandLocator extends HandLocator {
  // ...
  getItemIndex(item: MaterialItem, context: ItemContext): number {
    const { player, rules, index } = context
    if (item.location.player === player) {
      const hand = rules.material(MaterialType.Region).location(LocationType.PlayerRegionHand)
      const coins = hand.player(player)
      const sorted = orderBy(coins.getIndexes(), (index) => -getValue(hand.getItem(index).id))
      return sorted.indexOf(index)
    } else {
      return item.location.x!
    }
  }
}
```

The animation will not be "perfect", but the data will be secured and the cards will be ordered anyway for the player.

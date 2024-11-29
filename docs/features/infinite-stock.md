# Infinite stock

Sometimes, there are resources considered as infinite in games (coins, for instance).

During the game, infinite resources should not move from a "stock" item with a very high quantity.

Instead, they should be created and deleted all the time:
```typescript
this.material(MaterialType.Coins).createItem({ location: { type: LocationType.PlayerCoins, player }, quantity: 3 })
this.material(MaterialType.Coins).location(LocationType.PlayerCoins).player(player).deleteItem(2)
```

By default, when an item is created (or deleted) during the game, the animation will simply fade-in or fade-out the item.

To get a better experience, you should use a static item to display the stock,
and refer it as the stock location to get animations starting and ending from there:

```ts
class CoinsDescription extends TokenDescription {
  stockLocation = { type: LocationType.CoinsStock }
  staticItem = { quantity: 10, location: this.stockLocation }
}
```

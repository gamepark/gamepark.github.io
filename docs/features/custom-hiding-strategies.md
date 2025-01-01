# Custom Hiding strategies

The hiding strategies is the place where you describe what is hidden of the items, for each player.

For hiding strategies to work, the main rules class must extend [HiddenMaterialRules or SecretMaterialRules](step-by-step-example/hide-the-cards.md#hidden-or-secret).

For most games, you will only need to use the default hiding strategies:
- hideItemId: `() => ['id']` - always hides item.id.
- hideFront: `() => ['id.front']` - hides to "front" property of the item id. Useful when you have [cards with different backs](features/cards-with-different-backs.md).

The next hiding strategies only works inside SecretMaterialRules:
- hideItemIdToOthers: `(item, player) => item.location.player === player ? [] : ['id']` - hides the id except for the player matching item's location player: the item's owner.
- hideFrontToOthers: `(item, player) => item.location.player === player ? [] : ['id.front']` - combines both features of `hideFront` and `hideItemIdToOthers`.

If the default hiding strategies does not cover your use case, you can implement a custom hiding strategy.

If you implement `HiddenMaterialRules`, the API is the following function:

```typescript
type HidingStrategy = (item: MaterialItem) => string[]
```

Based on the item, you must return **the paths that you want to remove from the item** to everyone (players and spectators).

Here is an example using the item's rotation:

```typescript
const hideIdWhenRotated = (item: MaterialItem) => item.location.rotation ? ['id'] : []
```

:bulb: [Lodash unset function](https://lodash.com/docs/4.17.15#unset) is used to hide the paths returned by the hiding strategy.

If you implement `SecretMaterialRules`, you gain access to the player who will see the item:

```typescript
type HidingSecretsStrategy = (item: MaterialItem, player?: number) => string[]
```

:warn: The player can be undefined because it can be a spectator of the game. Make sure to always hide everything to the spectator!

Example:

```typescript
function hideIdToOthersWhenRotated(item: MaterialItem, player: number) {
  if (item.location.player === player) return []
  return item.location.rotation ? ['id'] : []
}
```

For a deeper understanding of how the hiding strategies works, have a look at the [Hiding data concepts](concepts/hiding-data.md).

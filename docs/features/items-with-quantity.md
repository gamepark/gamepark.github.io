# Items with quantity

Sometimes, the items comes in large number of identical items, like cubes of resources or coins.

Using a single item in the data model for each cube or coin would waste too much space. For these use case, we use the quantity field of the items:

```typescript
this.material(MaterialType.Coins).createItem({ quantity: 10, location: { type: LocationType.PlayerCoins, player } })
```

When moving or deleting items with quantities, you can specify the quantity you want to move or delete:

```typescript
material.moveItem({ type: LocationType.SomeLocation }, 5) // move 5
```

## Merging items

If an item is moved to the exact same location as another existing item, and both items have the same id, they can merge.

When 2 items merge, the moving item quantity is reduced to 0, and the other item's quantity is incremented with the moved item quantity.

This behavior is however dangerous when items id can be hidden (items without an id appears identical on the front, while they are not). For that reason, by default, items with a [hiding strategy](features/custom-hiding-strategies.md) cannot merge by default.

For this reason, you should not use the quantity field when creating items that can be hidden.

You can change the default behavior for merging by overriding `itemsCanMerge` in the main rules class.

## Money

Sometimes games offer different values of the same resource: for example, coins of value 1, 2 and 5.

It is nice to display all types of coins on the table, so we created a Money utility to make it easy to create the proper moves in the rules.

[Chateau Combo](https://github.com/search?q=repo%3Agamepark%2Fchateau-combo%20money&type=code) contains examples on how to use the utility.

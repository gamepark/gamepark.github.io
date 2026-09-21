# Item moves

All the moves that are movements of material are called **Material Moves**.

## Filter items

```typescript
this.material(MaterialType.Card)
  .location(LocationType.Hand)
  .player(this.player)
  .id<CardId>(id => isVirus(id))
```

- `this.material(type)` returns every item of that type. Each filter returns a **new** Material instance (it is immutable), so filters can be chained and a filtered instance can be kept and reused.
- Filters: `location`, `player`, `id`, `locationId`, `rotation`, `parent`, `selected`, `index` / `indexes`. Each takes either a value or a predicate.
- `filter((item, index) => boolean)` is the generic filter: the predicate receives the item itself.
- `sort`, `minBy`, `maxBy` and `limit` order and trim the items.
- Every move function below (`moveItems`, `deleteItems`, `shuffle`...) applies to the filtered items. `length`, `getItems()`, `getIndexes()` read them.

**Keep it a Material.** Do not turn a selection into an array of indexes, then rebuild one Material per index to read or move each item. Filter once and call the move function on the result. A helper that selects items should return a Material, not `number[]`.

```typescript
// ❌ Two `this.material(...)` calls per card: one to read it, one to move it
this.river.getIndexes()
  .filter(index => getCost(this.material(MaterialType.Card).getItem<CardId>(index).id) <= coins)
  .map(index => this.material(MaterialType.Card).index(index).moveItem({ type: LocationType.Discard }))

// ✅ One filter, one call
affordableCards() {
  return this.river.filter<CardId>(item => getCost(item.id) <= coins)
}
this.affordableCards().moveItems({ type: LocationType.Discard })
```

Indexes are only needed when the output itself is an index, for instance the data of a [custom move](features/custom-moves.md): `material.getIndexes().map(index => this.customMove(CustomMoveType.Copy, index))`.

## Create items

```typescript
material.createItem(item)
material.createItems(items)
```

- Each item is a Material Item.
- createItem returns a single move, createItems return one move per item.
- Items are created in the order of the array.
- Each created item will apply the [location strategy](features/location-strategies.md) matching its location type if any (addItem).
- If an item is created at the exact same location as another existing item, they will [merge](features/items-with-quantity.md#merging-items) if possible.
- If you need to create a lot of items at the same time during the game (not in the setup), use `material.createItemsAtOnce(items)` to create them in a single move.

## Move items

```typescript
material.moveItem(location)
material.moveItem(item => location)
material.moveItems(location)
material.moveItems(item => location)
```

- You first need to [filter the items](#filter-items) you want to move.
- You can either pass the location you want to move to, or a function that will return a location for each item. Useful if the new location depends on item's current location for instance.
- `moveItem` only moves the first item in the material instance filter, and returns a single move. It throws an error if there are no item in the filter.
- `moveItems` moves all the filtered items, and returns an array of moves. Empty filter produces empty array.
- You can use `moveItemsAtOnce` to move all the filtered items in a single move. Useful when moving a lot of items to prevent having too many moves or useless animations.
- The [location strategy](features/location-strategies.md) is applied to all item that moves (removeItem, moveItem or addItem).

## Delete items

```typescript
material.deleteItem()
material.deleteItems()
```

- You first need to [filter the items](#filter-items) you want to delete.
- Deleting items removes them entirely from the game state. If you pass a quantity, the quantity is subtracted from the item.
- `deleteItem` only deletes the first filtered items, and returns a single move. It returns an error if there is no filtered item.
- `deleteItems` deletes all the filtered items and returns an array of moves. Empty filter produces empty array.
- The [location strategy](features/location-strategies.md) is applied to all item that are deleted (removeItem).

## Shuffle

```typescript
material.shuffle()
```

- You first need to [filter the items](#filter-items) you want to shuffle.
- You can only shuffle items of the same Material Type.
- `shuffle` returns one Shuffle move with the list of the indexes of the items to shuffle.
- Shuffle swaps the indexes of the shuffle items. Therefore, items cannot be tracked by the players.

## Rotate items

```typescript
material.rotateItem(rotation)
material.rotateItem(item => rotation)
material.rotateItems(rotation)
material.rotateItems(item => rotation)
```

- Rotating items is a shortcut for Move items. It produces the same type of moves, there is no specific type of move.
- rotation is the `location.rotation` field, which can be anything (as long as it is serializable in JSON).

## Select items

```typescript
material.selectItem()
material.selectItems()
material.unselectItem()
material.unselectItems()
```

- Returns a `SelectItem` move or an array of `SelectItem` moves.
- Unselect item returns the same type of move.
- Used to update the `item.selected` field.

## Roll items

```typescript
material.rollItem()
material.rollItem(location)
material.rollItem(item => location)
material.rollItems()
material.rollItems(location)
material.rollItems(item => location)
```

- Returns one or an array of `RollItem` moves.
- `RollItem` will produce a randomized rotation output for `item.location.rotation`.
- By default, the randomized output is a number between 0 and 5 (6 face dice).
- To override the default behavior, you need to override `MaterialRules.roll`.
- You can use roll item for flipping coins.
- If no location is provided, only the rotation field is updated, the rest of the location will remain unchanged.

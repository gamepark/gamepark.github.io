# Items and locations

Before the Material approach was introduced, there was complete freedom on how to model the game state.

In most games, when a card for instance moved from a location to another location, the id of the card was removed from some array and added to another array.

Although this approach is very effective on the server side (it saves database space), it is a nightmare to animate on the client side, due to the item being removed from a place and added to another place.

To simplify and normalize every game adaptation, the material approach instead lists all the items (by type), and every item is responsible in holding its location in game.

# Items properties

```typescript jsx
type MaterialItem = {
  location: Location
  id?: any
  quantity?: number
  selected?: number | boolean
}
```

- The item has a mandatory [location](#location-properties).
- The id is optional, and describe variation in how the item looks compared to other items of the same type.
- The quantity is used when [items can merge](features/items-with-quantity.md) to save space
- The selected field sometimes used, when you need to mark items as selected by a player.

:bulb: the item does not hold its own type: instead, every item is indexed by its type in the game state:

```typescript jsx
type MaterialGame = {
  items: Record<MaterialType, MaterialItem[]>
  //...
}
```

# Location properties

```typescript jsx
type Location = {
  type: LocationType
  id?: any
  player?: Player
  parent?: number
  rotation?: any
  x?: number
  y?: number
  z?: number
}
```

- the type is the only mandatory field
- the id is a free field, used in last resort, when the type, player and parent field are not enough
- player: the owner of the location. Use it to identify the player the own the location, and the items there
- parent: the index of another item in the game. Use it if and only if the item is placed on top of another dynamic item.
- rotation: a free field to describe any king of rotation that the item can have at this location
- x, y, z: use it if you need a coordinates system at this location 

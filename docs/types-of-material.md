# Types of material

When you adapt a game for Game Park, you must split the material into different types:

```ts
// The material types in AracKhan wars
export enum MaterialType {
    BattleMat = 1, FactionCard, FactionToken, RoundTracker, RoundTrackerToken
}
```

Sometimes how to split is straightforward, sometimes the best option can be subtle to choose.

## Why do we split the material?

### 1. For performances

Items are stored in the game state this way:

```ts
items: {
    [MaterialType.A]
:
    [{item1, item2, ...}],
        [MaterialType.A]
:
    [{item1, item2, ...}],
        [MaterialType.A]
:
    [{item1, item2, ...}]
}
```

Having reasonable small arrays instead of 1 big array is better.

### 2. Because they do not look alike

When displaying the material, we have different tools for each "kind" of material (cards, tokens, boards, dices...).

### 3. Because it simplifies the code

The more you split, the less complex each material description will be.

## When splitting is a good idea

If the game has different kind of tokens, of cards, of resources, that never mix together,
it is best to split them into different material type:

```ts
export enum MaterialType {
    MyFirstKindOfCards = 1, MyOtherKindOfCards
}
```

## When splitting is not a good idea

If 2 kind of items (cards for examples), even with different image in the back, are located at the same place during the
game (player hand, same deck, same list...),
then it is a very bad idea to split them:

* On the rules side, you cannot [shuffle](item-moves.md#shuffle) different types of items together.
* On the app side, by default the locators only search for items of the same types when you need
  to [count](item-locator-api.md#countItems) how many items are in the same location.

## Mobile items versus static items

Some game contains items that never moves: boards are the most common example.

Although we could create them during the setup and therefor include them in the game state, we do not recommend doing so
as it takes unnecessary space in the database.

Instead, use [MaterialDescription staticItems](material-description.md#getstaticitems) to display them on the app side,
without ever creating them on the rules side.

```ts
// Arackhan Wars, BattleMatDescription.ts
export class BattleMatDescription extends BoardDescription {
    staticItem = {location: {type: LocationType.Table}}
}
```

## Infinite stock

Sometimes, there are resources considered as infinite in games (coins, for instance).

During the game, infinite resources should not move from a "stock" item with a very high quantity.

Instead, they should be [created](item-moves.md#create) and [deleted](item-moves.md#delete) all the time.

By default, when a item is created (or deleted) during the game, the animation will simply fade-in or fade-out the item.

To get a better experience, you should use a static item to display the stock,
and refer it as the [stock location](material-description.md#stocklocation) to get animations starting and ending from
there:

```ts
staticItem = {quantity: 10, location: ticketStockLocation}
stockLocation = ticketStockLocation
```

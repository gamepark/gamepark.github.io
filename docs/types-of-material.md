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
  [MaterialType.A]: [{item1, item2, ...}], 
  [MaterialType.A]: [{item1, item2, ...}], 
  [MaterialType.A]: [{item1, item2, ...}]
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
If 2 kind of items (cards for examples), even with different image in the back, are located at the same place during the game (player hand, same deck, same list...),
then it is a very bad idea to split them:

* On the rules side, you cannot [shuffle](item-moves.md#shuffle) different types of items together.
* On the app side, by default the locators only search for items of the same types when you need to [count](item-locator-api.md#countItems) how many items are in the same location.

## Mobile items versus static items


## Infinite stock
# Shuffling

Shuffling during the setup is very simple:
```typescript
this.material(MaterialType.Cards).location(LocationType.Deck).shuffle()
```
In the setup, moves are immediately executed.

However, shuffling during the game can be more tricky, because:

- You usually need to move the items before shuffling.
- Moves are prepared and returned to be executed later.
- Items must be shuffled inside a hidden location, otherwise players will know the result.

If you need to shuffle a discard to form a new deck, you can start by moving all the items at once:
```typescript
const discard = this.material(MaterialType.Card).location(LocationType.Discard)
return [discard.moveItemsAtOnce(LocationType.Deck)]
```

Then you can react to the movement of items to shuffle the deck:
```typescript
class SomeRule {
  afterItemMove(move: ItemMove) {
    if (isMoveItemsAtOnce(move) && move.itemType === MaterialType.Card) {
      const deck = this.material(MaterialType.Card).location(LocationType.Discard)
      return [deck.shuffle()]
    }
    return []
  }
}
```

This will work fine: items are shuffled after they are moved inside the deck.

However, you can take advantage of how the shuffling works: when the move is prepared, all the indexes of the items that need to be shuffled are listed inside the move.

So, you can prepare the shuffling move before the items are in the deck, as long as it is returned after the items are moved:

```typescript
const discard = this.material(MaterialType.Card).location(LocationType.Discard)
return [
  discard.moveItemsAtOnce(LocationType.Deck),
  discard.shuffled()
]
```

Of course, this last example is only equivalent to the first one if the deck was empty!

Sometimes, you need to draw new cards immediately after the shuffle. **This moves can never be prepared at the same time as the shuffle**: the new order is not known yet.

You need to react to the shuffle inside `afterItemMove`:

```typescript
class SomeRule {
  afterItemMove(move: ItemMove) {
    if (isShuffle(move) && move.itemType === MaterialType.Card) {
      const deck = this.material(MaterialType.Card).location(LocationType.Discard).deck()
      return deck.draw({ type: LocationType.PlayerHand, player: this.player }, 5)
    }
    return []
  }
}
```

:warning: A bad shuffle can compromise the game and remains easily unnoticed during the first tests, so ask for a code review everytime you need to shuffle inside the rules!

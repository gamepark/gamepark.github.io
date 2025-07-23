# Memory

Sometimes, the items and the ongoing rule are not enough to know exactly what is the state of the game. In a real time game, players remind some information (often for a short time).

To handle this use case, you can directly memorize, remind or forget any data:

```typescript
this.memorize(Memory.FirstPlayer, player)
this.remind<PlayerColor>(Memory.FirstPlayer)
this.forget(Memory.FirstPlayer)
```

:bulb: Memory is an enum used as a key to classify the memorized data: [see examples](https://github.com/search?q=org%3Agamepark+enum+Memory&type=code)

You can also memorize different data for each player under the same memory key:
```typescript
this.memorize(Memory.PlayerStuff, stuff, player)
this.remind<PlayerColor>(Memory.PlayerStuff, player)
this.forget(Memory.PlayerStuff, player) // deletes player's entry
this.forget(Memory.PlayerStuff) // deletes every players entries
```

The memorize function also accepts a function to set the new value based on previous value, and returns the new value:
```typescript
const newRound = this.memorize<number>(Memory.Round, (previousRound) => previousRound + 1)
```

You can get and set the memory inside the setup or any rule.

:warning: When you modify the memory, it is a **direct modification of the game's state**, unlike the moves that are returned to be executed later.
Therefore, it is impossible and forbidden to modify the memory inside `getPlayerMoves` functions for instance.
Also, as modifying the memory is a side effect, it must be used with care, and avoided if not necessary: it can be the source of nasty bugs.

A common pattern is to memorize stuff inside `onRuleStart` and forget them inside `onRuleEnd`.

Some complex game requires stacks of pending effects (like the piles in Magic): using the memory is perfect (and mandatory) for such use cases. [See examples](https://github.com/search?q=org%3Agamepark+Memory.Pending&type=code).

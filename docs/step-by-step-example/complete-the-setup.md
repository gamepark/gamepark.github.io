# Complete the Setup

## Moving items

You can use the Material API to filter on items you want to move, then move them around:

```typescript
this.material(MaterialType.Pawn).id(PawnColor.Blue).moveItem({ type: LocationType.Whatever })
```

## Shuffling

Shuffling is simple: use the Material API to filter on what you want to shuffle, and call the shuffle method.

```typescript
this.material(MaterialType.Card).location(LocationType.Deck).shuffle()
```

## Dealing cards

Dealing cards is tricky with the immutable Material API, so we create a deck API:

```typescript
const deck = this.material(MaterialType.Card).location(LocationType.Deck).deck()
deck.deal({ type: LocationType.PlayerHand, player }, 3)
deck.deal({ type: LocationType.PlayerHand, otherPlayer }, 3)
```

## Debugging the setup

When you run `game.new()` in the browser console, you start a new game, however it also reload the page.

If you put `console.log` instructions in the Setup, you need to check the "preserve logs" options in the console.

:bulb: We provide a few useful [console commands](features/console-commands.md)!

## The first rule

Finally, when the setup is done, and you have a satisfying layout for the game, you can start the first rule at the end of the setup:

```typescript
export class ColtSuperExpressSetup extends MaterialGameSetup {
  //...//
  start() {
    this.startPlayerTurn(RuleId.ThePlayerTurn, this.players[0])
  }
}
```

:bulb: If you start a rule that has automatic moves, they will be played automatically as a part of the setup too.


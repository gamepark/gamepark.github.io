# Player turn

## Start a player turn

The first kind of rule you might need, the most common one, is a player turn:

```typescript
export class ExampleRule extends PlayerTurnRule {
  
}
```

In a player rule, we expect to have, in the game state, `this.game.rule.player` equal to the active player. `this.player` is a shortcut for that.

To change the current rule, you need a "Rule move", which you can create this way:

```typescript
this.startPlayerTurn(RuleId.ThePlayerTurn, playerId)
```

This is what we usually do at the end of the setup.

:warning: To change the rule without changing the active player, you should use:

```typescript
this.startRule(RuleId.TheNewRule)
```

If you use `startPlayerTurn` instead of `startRule` inside the same player's turn, the undo feature will not be available.

## The legal moves

If the player has to choose what to do, you have to implement `getPlayerMoves`:

```typescript
export class ExampleRule extends PlayerTurnRule {
  getPlayerMoves(): MaterialMove[] {
    return this.material(MaterialType.Card)
      .location(LocationType.PlayerHand)
      .player(this.player)
      .moveItems({ type: LocationType.PlayArea })
  }
}
```

Using the Material API, you have to return every move that the player could actually do at this step of the game.

:bulb: Most of the time, the moves will be movements of material items, but it can also be [Rules moves](features/rule-moves.md) or [Custom moves](TODO).

## Move consequences

Most of the time, when a player plays a legal move, there are consequences:
* moving other items automatically
* changing the active player
* etc.

You can think of the consequences of a move, as everything that happens automatically because the players has no choice but to follow the rules at this point.

The most common way to react to the player moving an item is to implement "afterItemMove":

```typescript
export class ExampleRule extends PlayerTurnRule {
  // ...
  
  afterItemMove(move: ItemMove): MaterialMove[] {
    if (isMoveItemType(MaterialType.Card)(move) && move.location.type === LocationType.PlayArea) {
      return [this.startPlayerTurn(this.nextPlayer)]
    }
    return []
  }
}
```

:bulb: `this.nextPlayer` is a shortcut to get the player after the active player in the table order.

Alongside `afterItemMove`, you can also use `onRuleStart`, `beforeItemMove` and `onCustomMove` to create consequences.

[Learn more about Consequences](TODO)

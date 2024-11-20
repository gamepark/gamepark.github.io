# Simultaneous rules

## Legal moves per player

Sometimes there are phases in game where players can act simultaneously.

For such game phases, the RuleId is mapped to a `SimultaneousRule`:

```typescript
export class PlanStuffRule extends SimultanousRule {
  getActivePlayerLegalMoves(player: PlayerColor) {
    return this.material(MaterialType.Card).location(LocationType.PlayerHand).player(player)
      .moveItems({ type: LocationType.PlanArea, player })
  }
}
```

In such rules, you must return only the legal moves of the player passed in parameter.

## Deactivate a player

When a player is done, you have to end their turn, to stop their timer:

```typescript
export class PlanStuffRule extends SimultanousRule {
  // ...
  afterItemMove(move: ItemMove) {
    if (isMoveItemType(MaterialType.Card)(move) && move.location.type === LocationType.PlanArea) {
      return [this.endPlayerTurn(move.location.player!)]
    }
    return []
  }
}
```

## End of simultaneous rule

Once every player is done, you need to return the consequence, usually starting the next rule:

```typescript
export class PlanStuffRule extends SimultanousRule {
  // ...
  getMovesAfterPlayersDone() {
    return [this.startPlayerTurn(RuleId.SomePlayerTurn, somePlayer)]
  }
}
```

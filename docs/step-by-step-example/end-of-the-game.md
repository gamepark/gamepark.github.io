# End of the game

## Triggering the end

When a condition is met to end the game, you need to return a `endGame` move as a consequence:

```typescript
export class SomeRule extends PlayerTurnRule {
  // ...
  
  afterItemMove(move: ItemMove) {
    if (someEndOfGameCondition) {
      return [this.endGame()]
    }
    return []
  }
}
```

This will remove `rule` from the game state, marking the end of the game.

## Identify the winner

If the game is a competitive game, in the [main rule class](https://github.com/gamepark/board-game-template/blob/main/rules/src/GameTemplateRules.ts) you need to implement the `CompetitiveScore` interface:

```typescript
export class GameTemplateRules extends MaterialRules<PlayerColor, MaterialType, LocationType>
  implements TimeLimit<MaterialGame<PlayerColor, MaterialType, LocationType>, MaterialMove<PlayerColor, MaterialType, LocationType>, PlayerColor>,
    CompetitiveScore<MaterialGame<PlayerColor, MaterialType, LocationType>, MaterialMove<PlayerColor, MaterialType, LocationType>, PlayerColor> {
  // ...
  
  getScore(player: PlayerColor) {
    return // the score of the player based on the state of the game over
  }
}
```

## Tie breakers

If the rules have tie-breakers, you can implement `getTieBreaker`:

```typescript
export class GameTemplateRules {
  // ...

  getTieBreaker(tieBreaker: number, player: PlayerColor) {
    if (tieBreaker === 1) {
      return // the first tie breakers
    } else if (tieBreaker === 2) {
      return // another tie breaker?
    }
    return
  }
}
```

It allows for multiple cascading tie-breaking conditions.

:warning: Make sure to return `undefined` when all the tie-breakers are exhausted to prevent an infinite loop.

## Rank players

If you implement `CompetitiveScore`, players are automatically ranked by their scores and the tie-breakers.

However, some competitive games does not have a score feature. In that case, you can implement `CompetitiveRank` instead of `CompetitiveScore`:

```typescript
export class GameTemplateRules extends MaterialRules<PlayerColor, MaterialType, LocationType>
  implements TimeLimit<MaterialGame<PlayerColor, MaterialType, LocationType>, MaterialMove<PlayerColor, MaterialType, LocationType>, PlayerColor>,
    CompetitiveRank<MaterialGame<PlayerColor, MaterialType, LocationType>, MaterialMove<PlayerColor, MaterialType, LocationType>, PlayerColor> {
  // ...

  rankPlayers(playerA: PlayerId, playerB: PlayerId) {
    return // a positive number if B beats A, a negative number if A beats B, 0 in case of an equality
  }
}
```

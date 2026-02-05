# Final scoring

You can display a final scoring in the result dialog that is automatically opened when a game is over.

You need to implement a Scoring class and reference it inside the GameProvider:

`<GameProvider ... scoring={new MyGameScoring()}/>`

The class looks like this:

```typescript jsx

enum ScoringKeys {
  SomeSubScoreType,
  SomeOtherSubScoreType
}

export class MyGameScoring implements ScoringDescription {
  getScoringKeys(rules: MyGameRules) {
    return getEnumValues(ScoringKeys)
  }

  getScoringHeader(key: ScoringKeys) {
    switch (key) {
      case ScoringKeys.SomeSubScoreType:
        return <Trans defaults="game-over.some.score.type" />
      //...
    }
  }

  getScoringPlayerData(key: ScoringKeys, player: number, rules: MyGameRules) {
    switch (key) {
      case ScoringKeys.SomeSubScoreType:
        return <Trans defaults="game-over.some.score.type" />
      //...
    }
  }
}
```

Have a look at [some examples from existing games](https://github.com/search?q=org%3Agamepark+%22implements+ScoringDescription%22&type=code).

## Score pad

You can include a score pad on the table, and fill it with the final scoring at the end of the game.

It is very useful to display a score pad as the linked help dialog can help players remember how the scoring works in the game.

The score pad is a Material Type, and all the writing inside are Locations.

Have a look at [Les Jardins Suspendus (Hanging gardens)](https://github.com/gamepark/les-jardins-suspendus/blob/main/app/src/material/ScorePadDescription.ts) for an example.

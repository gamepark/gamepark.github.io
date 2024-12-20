# Conditional consequences

Sometimes, when you build the consequences of a move, some consequences depends on what happens in previous consequences:

```typescript
class SomeRule extends PlayerTurnRule {
  // ...
  
  afterItemMove(move: ItemMove) {
    const moves: MaterialMove[] = []
    if (isMoveItemType(MaterialType.Something)(move)) {
      moves.push(...someHelperForComplexRule.doStuff())
      if (someConditionAboutGameState) {
        moves.push(goToRule1)
      } else {
        moves.push(goToRule2)
      }
    }
    return moves
  }
}
```

Here, we want to start a different rule depending on the state of the game.

**However, the moves prepare before (inside `someHelperForComplexRule.doStuff`) are not executed yet!**

Because of that, the state of the game is not updated, so if `someConditionAboutGameState` depends on it, **the code will not work**.

#### Solutions

- If the previous condition always return exactly one move, you can react to that move itself (afterItemMove, onCustomMove...).
- You can start a new RuleId, and process the condition inside that new rule (onRuleStart): here, the moves will have been executed.
- You can create a custom move and react to that move. This solution is very similar to the previous one.
- You could try to get the right state depending on the moves prepared, but we generally advise against it as it often produces complex code.


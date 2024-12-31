# Identify the rules

## Keep the code organized

A board game can have a lot of rules: we need to organized them in multiple files so that the code remains easy to write and maintain.

The [RuleId file](https://github.com/gamepark/board-game-template/blob/main/rules/src/rules/RuleId.ts) is here to organize the code in small rules steps.

Inside the main Rules file, each Rule id is mapped with a single rule class that contains the logic of the game at this specific step:

```typescript
export class ColtSuperExpressRules {
  rules = {
    [RuleId.Scheming]: SchemingRule,
    [RuleId.Shooting]: ShootingRule,
    [RuleId.RoundEnd]: RoundEndRule
  }
}
```

:bulb: By convention, the rules classes are name after the RuleId + "Rule" suffix.

Inside the game state, the current rule is always stored:
```
{
  players: [1, 2],
  items: {...},
  rule: { id: 1, player: 1 }
}
```

No rule in the game state means the game is over.

## How to split the rules

Depending on the game, it can be simple, or more tricky, to choose the best way to split the rules.

The Rule id is also used to display [the header](step-by-step-example/write-the-headers.md) in the game, on top of the screen: a short message to inform "who does what". Therefore, it is convenient to have 1 rule for each different header you want to show during the game.

Sometime, you might not want to split one rule because the 2 rules share a lot of common code. However, it is very easy to share the code by inheritance (`RuleA extends RuleB`) or composition, using a third file for common features.

Also, rules can contain only automatic steps, and not even move any material. A rule can only be a small step that reroute to the correct step.

So, do not hesitate to split the code in a lot of very small rules.
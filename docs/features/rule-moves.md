# Rule moves

Rule moves are the moves that will change the current [Rule state](step-by-step-example/identify-the-rules) of the game.

The rule state has this data structure:
```typescript
export type RuleStep<Player extends number = number, RuleId extends number = number> = {
  id: RuleId
  player?: Player
  players?: Player[]
}
```

- ID is the current RuleID, which drives the current rule class and the header.
- player is the current active player
- players are the current active players for simultaneous steps

There are 5 different types of rule moves:

## Start player turn

`startPlayerTurn` will start the turn of a single player. It changes both the rule id and the active player:

```typescript jsx
this.startPlayerTurn(RuleId.SomeRule, player)
```

:warning: do not use `startPlayerTurn` if the player does not change, as it will otherwise prevent any legal undo. 

## Start rule

`startRule` starts a new Rule ID but keeps the active player (or players) as it is:

```typescript
this.startRule(RuleId.SomeRule)
```

:bulb: `startRule` does not prevent the undo, so it should be used all the time when the active player does not change.

## Start simultaneous rule

`startSimultaneousRule` start a new Rule ID where multiple players are active at the same time.

By default, all the players will be active:

```typescript
this.startSimultaneousRule(RuleId.SomeRule)
```

If only a subset of the players are active (it can be only 1 player as well), you need to specify the players:

```typescript
this.startSimultaneousRule(RuleId.SomeRule, activePlayers)
```

## End player turn

Inside a simultaneous rule, `endPlayerTurn` will remove a player from the active players list:

```typescript
this.endPlayerTurn(player)
```

This is the only rule move that does not change the Rule ID. Have a look at [Simultaneous Rules](step-by-step-example/simultaneous-rules.md) for more details.

## End game

`endGame` will end the game by removing the current Rule ID from the game state: `this.endGame()`.
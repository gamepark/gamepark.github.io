# Consequences

## Automatic moves

We could implement the rules of a board game without taking care of the user interface. Players play moves, it produces a new game state, until the game is over.

However, in a board game, there are very often some things that the players must do without any choice. Like, paying gold after choosing a card to buy, refilling a river of cards, shuffling a deck, etc.

If the user interface does not know what happens in which order after a player's decision, we cannot animate what is happening step-by-step.

We could reimplement the rules for the front-end, but it would be a huge waste of time.

For that reason, we design the moves as "small and simple events" in the game. One move only updates very little information in the game state.

A move played by a player can have consequences. Consequences are other moves that are played automatically after the move.

In the first version of the framework, developers had to implement the function `getAutomaticMoves` and return the consequences, given the current game state, here.

However, this approach, though it maximize the performances, was not always easy to code:
- any direct update of the game state inside `getAutomaticMoves` was a mistake, because the function was only ever called once (not in replays, etc.)
- it was often not convenient to store what just happened in the state only to return the proper automatic moves immediately after.

For that reason, we deprecated this function and now encourage directly returning the consequences when the move is played.

## Consequences

Everytime a move is played, the consequences that should immediately follow the move must be returned by the `play` function.

Inside material rules, you return consequences using the functions `afterItemMove`, `beforeItemMove`, `onCustomMove` or `onRuleStart` (the play function delegates the job to these function).

When a list of consequences is returned, the consequences are played immediately by the server, however the client will apply the consequences step-by-step, producing an animation each time it is required.

A consequence can also return other consequences. The new consequences always have the highest priority, so they will happen before any pending consequences that were already in the line.

Example:
- Move A returns consequences [B, C].
- Move B is applied and returns consequences [D].
- D is applied before C. Any consequence of D is also applied before C, no matter what.

## Actions

An `Action` is the data structure that represents a move and all its consequences:

```typescript
type Action = {
  player: number
  move: Move
  consequences: Move[]
}
```

If a player uses the undo button, it will cancel his last **action**: his last move and all that move's consequences.

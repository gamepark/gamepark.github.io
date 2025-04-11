# Moves history

In many game, it is nice to be able to see what happened from the beginning. This move history (or game logs), can be implemented base on the moves played and the state of the game when that move was played.

All the logs appears inside the top-left menu. They can also appear on top of the table for a short period of time after they are played (we call that "live logs").

In `index.tsx`, add the history in the GameProvider:

```typescript jsx
<GameProvider logs={new MyGameHistory()}/>
```

MyGameHistory is a class that implements LogDescription: see [an example in Slide (Dékal)](https://github.com/gamepark/dekal/blob/main/app/src/history/DekalHistory.ts).

`getMovePlayedLogDescription` returns a `MovePlayedLogDescription`, or `undefined` if you do not want a log line to appear for this move.

A `MovePlayedLogDescription` contains:
- a Component, which is the component that will display the log line
- a player, if you want that player's avatar to show at the beginning of the log line
- a depth, if you want an arrow + tab to display, if the move is a direct consequence of the previous log line
- css: extra css to style the log line
- liveCss: extra css that will only apply to the log line displayed on top of the table

## Best practices for writing log texts

- Use present tense (not past): "The round ends"
- Use only the 3rd person: "John plays a card" (even if John is ready that text)

In the headers, it is important to use the first person to address the player (You must play a card), however in the moves history it is not, so we save time & bandwidth.

The keys `history.game.start` & `history.game.over` are reserved and do not need to be translated, they are in the common translation file.

# Tutorial AI

After explaining the rules in the scripted part of the tutorial, the player is free to complete the game against a bot.

By default, the bot selects random legal moves to play. In some games, it works, but other games have moves that are obviously bad, which can hinder the player's experience.

In such case, a tutorial AI can be created. It plays the opponents of the tutorial, and the bots you start with [`game.bot()`](features/console-commands.md) in development. It never runs in online games.

## API

Pass the AI to the GameProvider in `app/src/main.tsx`:
```typescript jsx
<GameProvider
  {/* ... */}
  tutorial={new Tutorial()}
  ai={TutorialAI}>
  <App/>
</GameProvider>
```

The AI is a `GameAI`:
```typescript jsx
type GameAI<Game = any, Move = any, PlayerId = any> = (game: Game, playerId: PlayerId) => Promise<Move[]>
```

Given a game state and a player id, the AI must return the moves to play at this step. When it returns several moves, each one must be legal once the previous one is played.

:warning: The AI receives the **full game state**, including what the player cannot see. It must only read what its player is allowed to know.

## Performance

The GameAI returns a promise, because some AI might take time to resolve their moves. Because Javascript is single threading, the UI will be unresponsive during the calculations.

If the AI is very short to resolve, you can resolve the promise synchronously and block the UI, like in [Chateau Combo](https://github.com/gamepark/chateau-combo/blob/main/app/src/tutorial/TutorialAI.ts).

However, if you have a longer AI, you need to set up a worker like in [AracKhan Wars](https://github.com/gamepark/arackhan-wars/tree/main/app/src/tutorial).

## Examples

[Greylune](https://github.com/gamepark/greylune/tree/main/rules/src/ai) and [Panorama](https://github.com/gamepark/panorama/tree/main/rules/src/ai) have strong bots: an evaluation of the position, a short search, and an arena that measures each change on simulated games.

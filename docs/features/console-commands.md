# Console commands

In the console of you browser, in development mode, we provide a few commands to simplify testing the game.

## Start a new game

:warning: starting a new game will erase the current game from your local storage

- Start a new game with 2 players: `game.new()`
- Start a new game with 4 players: `game.new(4)`
- Start a new game with custom [game options](features/game-options.md): `game.new({ players: [{id: 1}, {id: 3}], expansion: true })`

## Change active player

- Take control of player with id 2: `game.changePlayer(2)`
- Watch as a spectator: `game.changePlayer()`

## Make other players play

Because the application knows all the [legal moves](step-by-step-example/player-turn.md), we can ask other players to play random legal moves:

- to activate random bots: `game.bot()` (or `game.monkeyOpponents()`)
- to deactivate random bots: `game.bot(false)` (or `game.monkeyOpponents(false)`)

:bulb: to watch a game played by bots, you can watch as a spectator then activate the bots! They will stop after 100 moves (a security), but you can restart the bots multiple times.

## Undo the last moves

You can always undo the last moves using the console: `game.undo()`.

It will undo the last move, whichever played that move.

You can also undo multiple moves at once: `game.undo(5)`

## Change the animations speed

- Make animations 3 times faster: `setAnimationsSpeed(3)`
- Make animations 5 times slower: `setAnimationsSpeed(0.2)`

## Access the game and moves state

`game.state` and `game.moves` will display the state of the game and all the moves played from the beginning.

It is the state on the server side. These commands are for advanced usage as you should never manipulate the game state and the moves directly, but only through the Material API.

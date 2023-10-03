# Players identifiers

The player id is the **unique number** that links a player in your game with the Game Park user.

## Games with player colors

If your game offers a way to identify the players (it is often a color), go to `/rules/src/PlayerColor.ts` and change the values with the colors of your game:
```ts
export enum PlayerColor {
  Blue = 1, Red, Green, Yellow
}
```

:bulb: We start the enum at 1 instead of 0 because 0 is a [Javascript falsy value](https://developer.mozilla.org/en-US/docs/Glossary/Falsy). It could trick you into nasty bugs.

:warning: You should rename the file and the Enum if your game has something else than a "color" to identify the players.

You will also need to update the [Game Options](game-options.md).

## Games without any player identifier

If your game does not provide any way to identify the players, you can:

1. Replace any reference to `PlayerColor` in the code with the `number` type.
2. Remove the PlayerOptions from the [Game Options](game-options.md).
3. Delete `/rules/src/PlayerColor.ts`

Game Park will automatically set a sequence of numbers (1, 2, 3, 4, ...) as the player ids.

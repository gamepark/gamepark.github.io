# Game setup

When a new game is started, the Game setup is called to prepare the **initial game state**.

The GameSetup file can be found in `/rules/src/GameSetup.ts`.

```ts
export class GameSetup extends MaterialGameSetup<PlayerColor, MaterialType, LocationType, GameOptions> {
  setupMaterial(_options: GameOptions) {
  }

  start() {
    return { id: RuleId.PlayerTurn, player: this.game.players[0] }
  }
}
```

## Setup the Material

In this function, you can create and move the material of the game into it's starting position.

*TODO: explain the API and use Black Lady as an example*

Use `this.material(MaterialType.Whatever)` to access the [Material API](material-api.md).

:information_source: during the game setup, using functions such as `createItem`, `moveItem`... from the Material API **immediately updates the Game state**.
Do not get use to it! It will no longer be the case once the game is started, as explained in the Material API.

:bulb: setupMaterial received the [Game options](game-options.md) in parameter. If you do not have any custom game options you can delete the parameter.

## Rules with choices during the setup

On Game Park, everything happening in the Game Setup is automatic.

Some game have choices that players make during the setup. You cannot implement that inside the GameSetup, because those choices are meaningful game actions that must be part of the game history.

In this case, just setup as much as you can in the game setup, and create a first RuleId such as "RuleId.Setup".

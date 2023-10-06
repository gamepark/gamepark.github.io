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

In this file, you will typically create the material (as you would put it out of the game box) and place it where it belongs.

Then, you will start the first "rule", which is usually the first time a player has to choose what to do.

:bulb: [Static items](types-of-material.md#mobile-items-versus-static-items) (items that do not move during the game, like a board) should not be included in the game state (to
save storage space).

:warning: Every time you change the setup, you must [start a new game](console-commands.md#new) to test the result.

## Rules with choices during the setup

On Game Park, everything happening in the Game Setup is automatic.

Some game have choices that players make during the setup. You cannot implement that inside the GameSetup, because those choices are meaningful game actions
that must be part of the game history.

In this case, just setup as much as you can in the game setup, and create a first RuleId such as "RuleId.Setup".

## API

### setupMaterial

You must implement this function to prepare the initial state of the game.

When this function is called, `this.game` is already populated with `players: [1, 2, ...]`.

You have to create some [Material types](types-of-material.md) and [Location types](types-of-locations.md) prior to implementing setupMaterial.

#### Arguments

1. `options: GameOptions`: the [options of the game](game-options.md), chosen by the players or the matchmaking algorithm
   (the parameter can be deleted if you do not have any specific option).

#### Returns

`void`

#### Example

For any typical card game, we create the cards inside the deck, then we shuffle the deck:

```ts
  setupMaterial()
{
  this.material(MaterialType.Card).createItems(cards.map(card => ({ id: card, location: { type: LocationType.Deck } })))
  this.material(MaterialType.Card).shuffle()
}
```

:information_source: during the game setup, using functions such as `createItem`, `shuffle`... from the Material API **immediately updates the Game state**.
Do not get use to it! It will no longer be the case once the game is started, as explained in the Material API.

### start

You must implement this function to return the first [rule step](splitting-the-rules.md) of the game.

This function is called after `setupMaterial`, so `this.game` will contain every update you already made there.

#### Arguments

1. `options: GameOptions`: the [options of the game](game-options.md)

#### Returns

`RuleStep`: the first [rule step](splitting-the-rules.md) of the game.

#### Example

Start the rule named `PlayerTurn`, and activate the first player:

```ts
  start()
{
  return { id: RuleId.PlayerTurn, player: this.game.players[0] }
}
```

:information_source: the players order is randomized before the game is started, you do not have to randomize them during the setup.

#### Arguments

1. `options: GameOptions`: the [options of the game](game-options.md)

#### Returns

`Game`: the initial state of the game.

### material

This function creates a [Material](material-api.md) instance that will allow you to directly update `this.game.items`.

:warning: During the setup, calling functions like `createItems` or `moveItems` on the Material API will immediately update the game state.
It will not be the case after the setup, in the [rules classes](the-rules-api.md#material).

#### Arguments

1. `type: MaterialType`: the [Type of Material](types-of-material.md) that you want to manipulate.

#### Returns

A [Material](material-api.md) instance referencing all the items of the specified type that exists in the [game](#game) when the function is called.

#### Examples

Create a deck of cards:

```ts
this.material(MaterialType.Card).createItems(cards.map(card => ({ id: card, location: { type: LocationType.Deck } })))
```

Deal the 10 top cards of the deck to a player:

```ts
this.material(MaterialType.Card).location(LocationType.Deck).sort(item => -item.location.x!).limit(10).moveItems({
  location: {
    type: LocationType.Hand,
    player
  }
})
```

### getMemory

Access to the [Memory API](memory-api.md), which allows to memorize, remind and forget any value you need.

During the setup, you should only need to memorize, using the [memorize](#memorize) shorthand function.

#### Arguments

1. `player?: Player`: if you need to memorize different values for each player, you can pass the player here. Leave it empty for the global game memory.

#### Returns

A GameMemory or a PlayerMemory instance.

### memorize

Shorthand access to `this.getMemory(...).memorize(...)`

#### Arguments

1. `key: keyof any`: the unique key where the value will be stored.
2. `value: any`: value you need to store (it must be compatible
   with [JSON serialization](https://developer.mozilla.org/en-US/docs/Web/JavaScript/Reference/Global_Objects/JSON/stringify)).
3. `player?: Player`: if you need to memorize different values for each player, you can pass the player here. Leave it empty for the global game memory.

#### Returns

`void`

#### Example

Memorize the current round:

```ts
this.memorize(Memory.Round, 1)
```

Initialize a score to 0 for each player:

```ts
for (const player of this.players) {
  this.memorize(Memory.Score, 0, player)
}
```

### game

This property contains the object that will be provided as the initial game state.

You should not have to manipulate this property directly, but through the helper functions [material](#material) and [memorize](#memorize)

#### initial value

`{ players: [], items: {}, memory: {} }`

`players` is filled with the [players identifiers](players-identifiers.md) inside the [setup function](#setup)

### players

You can use `this.players` and a shorthand property for `this.game.players`

### setup

The base function called by Game Park server.

It has a default implementation that should work for every games following the [Material approach](material-approach.md):

```ts
  setup(options
:
Options
):
MaterialGame < P, M, L > {
  this.game = { players: getPlayerIds(options), items: {}, memory: {} }
  this.setupMaterial(options)
  this.game.rule = this.start(options)
  return this.game
}
```

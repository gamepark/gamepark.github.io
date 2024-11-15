# Create items

## The game setup

[The game setup](https://github.com/gamepark/colt-super-express/blob/main/rules/src/ColtSuperExpressSetup.ts) is the file in which you prepare the initial state of the game, before any player takes a decision.

Here is how you can create an item in the setup:

```typescript
export class ColtSuperExpressSetup extends MaterialGameSetup {
  setupMaterial() {
    this.material(MaterialType.SomeStuff).createItem({ location: { type: LocationType.SomeSpace } })
  }
}
```

`this.material` gives access to the Material API. You will use this API all the time to search and manipulate the items of the game.

## Start a new game

:warning: Everytime you change the setup, you will only see the difference after starting a new game.
The game state is store into your browser's locale storage.

If you open your browser console and write `game.state`, you will see the game state:
```
{
  items: {}
  players: [1, 2]
  ...
}
```

The item you just created in the setup is not here as long as you do not run `game.new()` in the console.

Then, `game.state` with show:
```
{
  items: {
    1: [{location: {type: 2}}]
  }
  players: [1, 2]
  ...
}
```
1 and 2 matching your MaterialType and LocationType respectively.

:bulb: `game.new(3)` will state a 3-players game. `game.new(options)` will start a game with any options.

## Create multiple items

You can use `createItems` to create multiple items:

```typescript
export class ColtSuperExpressSetup extends MaterialGameSetup {
  setupMaterial() {
    this.material(MaterialType.SomeStuff).createItems(this.players.map(player => ({ location: { type: LocationType.SomeSpace, player } })))
  }
}
```
Here we create one item per player in the game. Each item has a `location.player` field equal to one player id:
```
{
  items: {
    1: [
      {location: {type: 2, player: 1}},
      {location: {type: 2, player: 2}}
    ]
  }
  players: [1, 2]
  ...
}
```
# The first legal moves

On Game Park, when it is a player's turn, you must list all the moves this player is allowed to do.

If your first rule is a player's turn:
```ts
// GameSetup.ts
start()
{
  return { id: RuleId.SomePlayerTurn, player: this.game.players[0] }
}
```

You must map the RuleId to a class that implements [PlayerTurnRule](player-turn-rules.md):
```ts
// GameRules.ts
import { SomePlayerTurnRule } from './rules/SomePlayerTurnRule'

// [...]
rules = {
  [RuleId.SomePlayerTurn]: SomePlayerTurnRule,
}

// SomePlayerTurnRule.ts
export class ChooseStartPlayerRule extends PlayerTurnRule<PlayerId, MaterialType, LocationType> {
  getPlayerMoves() {
    return [] // the player's legal moves
  }
}
```

There are [many good reasons to list all the legal moves](legal-moves.md). One of them is that if you allow the player to move some item, the item will automatically become draggable in the application:

```ts
getPlayerMoves() {
  return this.material(MaterialType.Card).location(Location.Hand).player(this.player).moveItem({ location: { type: LocationType.Table } })
}
```

Once it is draggable, you can [describe the destination location](location-description.md) to make it droppable.

The move will automatically be played!

Then you can [react to the move in various ways](moves-consequences.md) for the game to go on:

```ts
afterItemMove() {
  return this.rules().startPlayerTurn(RuleId.SomePlayerTurn, this.nextPlayer)
}
```

That's all you have to do to implement a game:
* Create [the material](material-api.md) and its [display locations](item-locator-api.md);
* List the [legal moves](the-rules-api.md) at each step of the game;
* List the [consequences](moves-consequences.md) when a move is played.

Happy coding!

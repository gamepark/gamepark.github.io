# Custom moves

## Data structure

Sometimes, [Item moves](features/item-moves.md) and [Rule moves](features/rule-moves.md) are not enough. Custom moves allows to create any type of move that you need.

The data structure of custom move is free:
```typescript
export type CustomMove<Type extends number = number> = {
  kind: typeof MoveKind.CustomMove
  type: Type
  data?: any
}
```

The `data` property accepts any type, however **it must be serializable in JSON**. You should only use number, string, booleans, arrays and object types.

## Usage

By convention, we use to create an enumeration of the custom move types in the games that requires custom moves:
```typescript
enum CustomMoveType {
  Pass = 1, ChoosePlayer, SomeOtherStuff, Etc
}
```

You can create a custom move using `this.customMove(CustomMoveType.SomeOtherStuff, data)`.

Because of their nature, custom moves will not be triggered by moving some material in the interface. You need to add some button in the header:

```typescript jsx
const pass = useLegalMove(isCustomMoveType(CustomMoveType.Pass))
return <Trans defaults="header.play-or-pass" components={{
  pass: <PlayMoveButton move={pass}/>
}}/>
```

Inside the rule class, you must react to the custom using `onCustomMove`:
```typescript
class SomeRule extends PlayerTurnRule {
  onCustomMove(move: CustomMove): MaterialMove[] {
    if (move.type === CustomMoveType.Pass) {
      return //...
    }
    return []
  }
}
```

## Use case: Pass

The most common use case for Custom moves is [when a player says "I pass"](https://github.com/search?q=org%3Agamepark%20CustomMoveType.Pass&type=code).

Sometimes the next rule is straightforward and the Rule move (like `startPlayerTurn`) can be used as the "pass move".

However, in many case there are multiple things to do before the next rule starts if the player passes. In that case, the best solution is to use the custom move, and to return all the moves that need to happen as a result.

## Use case: Choose player

Another common use case for custom move is when you need to [choose a player](https://github.com/search?q=org%3Agamepark+CustomMoveType.ChoosePlayer&type=code) (as the target for an effect for example). Use the data field as the player id:

```typescript
return this.game.players.map(player => this.customMove(CustomMoveType.ChoosePlayer, player)) // choose any player
```

:bulb: It is often better to create a generic CustomMoveType.ChoosePlayer and reuse it for different effects than creating many different types.

## Enable drag and drop for a custom move

You can trigger a custom move using drag and drop.

First you need to tell that an item can be dragged when the custom move is available. You do it by overriding `canDrag` in the Material Description.

Here is an [example from AracKhan Wars](https://github.com/gamepark/arackhan-wars/blob/a05f6c6069a78226df134ec84ce271f6d989a2e2/app/src/material/FactionCardDescription.tsx#L467):

```typescript jsx
export class FactionCardDescription extends CardDescription {
  //...
  canDrag(move: MaterialMove, context: ItemContext) {
    if (isCustomMoveType(CustomMoveType.Attack)(move)) {
      return move.data.card === context.index
    }

    return super.canDrag(move, context)
  }
}
```

Then you need to define where the item can be dropped. Override `getMoveDropLocations` in your Material Description:

```typescript jsx
export class FactionCardDescription extends CardDescription {
  getMoveDropLocations(context: ItemContext, move: MaterialMove) {
    if (isCustomMove(move) && move.type === CustomMoveType.Attack) {
      const targets = move.data.targets
      return targets.map(target => ({ type: LocationType.Card, parent: target }))
    }
    return super.getMoveDropLocations(context, move)
  }
}
```

This method returns the locations where the dragged item can be dropped to trigger the custom move. The framework will highlight these locations and allow the drop.

## More complex use cases

Search for "[enum CustomMoveType](https://github.com/search?q=org%3Agamepark+enum+CustomMoveType&type=code)" in our GitHub repositories to find a lot of other use cases!

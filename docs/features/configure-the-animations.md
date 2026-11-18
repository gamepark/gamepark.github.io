# Configure the animations

Animations are configured with a `MaterialGameAnimations` instance, passed to the `GameProvider` in `app/src/main.tsx`:

```typescript jsx
<GameProvider
  {/* ... */}
  animations={gameAnimations}
>
```

## Durations

```typescript
export const gameAnimations = new MaterialGameAnimations<PlayerColor, MaterialType, LocationType>()

gameAnimations
  .configure((move) => isMoveItemType(MaterialType.Pawn)(move) && move.location.type === LocationType.Board)
  .duration(500)

gameAnimations.forMaterial(MaterialType.Coin).duration(200)

gameAnimations.forRule(RuleId.Scoring).duration(300)

gameAnimations.configure(isShuffle).skip()
```

- The duration is expressed in **milliseconds**. It sets both the time before the next move is played, and the CSS animation duration.
- The first configuration matching a move applies: write the most specific ones first.
- The predicate of `configure` also receives a context, with `context.rules` and `context.player`.
- `gameAnimations.defaults()` configures the moves that nothing else matches.

:warning: Older games use `gameAnimations.when().move(predicate).duration(0.5)`, where the duration is in **seconds**. It still works, but prefer `configure`.

## Trajectories

You can change the arc an item follows on its way:

```typescript
gameAnimations.forMaterial(MaterialType.Pawn).duration(500).arc(5) // rises up to 5 centimeters
gameAnimations.forMaterial(MaterialType.Card).duration(300).flat() // no elevation
```

Use waypoints to make an item pass through a given place, for example to show a card for a moment before it reaches a hand, or to send it behind the panel of a player whose area is not displayed:

```typescript
gameAnimations
  .configure(isDrawnCard)
  .duration(1000)
  .trajectory((context, move) => ({
    waypoints: [{ at: 0.5, locator: revealLocator, location: { player: context.player } }]
  }))
```

- `at` is the position in the animation, from 0 to 1.
- A waypoint is placed with a `locator` and its `location`, with absolute `coordinates`, or with an `offset` from the straight line.
- It can also set the `elevation`, the `rotation` and the `easing` at that point.
- `.via(locator, 0.5)` and `.through(...waypoints)` are shortcuts.

An item coming from, or going to, a place where it is not displayed can fade instead of popping in or out. Each value is a fraction of the animation:

```typescript
gameAnimations.configure(isHiddenMove).trajectory({ fade: { in: 0.2, out: 0.2 } })
```

## Animations speed

In development mode, you can speed up the animations in the browser console: `game.setAnimationsSpeed(2)` (2x faster).

You can also slow the animations: `game.setAnimationsSpeed(0.5)` (2x slower).

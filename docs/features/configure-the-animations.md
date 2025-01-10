# Configure the animations

Animations time can be configured in `app/src/index.tsx`:

```typescript jsx
<GameProvider
  {/* ... */}
  animations={gameAnimations}
>
```

The configuration of the animation is made using the `MaterialGameAnimations` class. Here is an example from [Faraway](https://github.com/gamepark/faraway/blob/main/app/src/animation/FarawayAnimations.ts):
```typescript jsx
export const farawayAnimations = new MaterialGameAnimations()

farawayAnimations.when()
  .move((move) => isMoveItemType(MaterialType.Region)(move) && move.location.type === LocationType.Region)
  .duration(0.5)

farawayAnimations.when()
  .move((move) => isMoveItemType(MaterialType.Region)(move) && move.location.type === LocationType.RegionDiscard)
  .duration(0.5)

farawayAnimations.when()
  .move((move) => isMoveItemType(MaterialType.Sanctuary)(move)
    && (move.location.type === LocationType.SanctuaryDeck || move.location.type === LocationType.PlayerSanctuaryHand))
  .duration(0.3)

farawayAnimations.when()
  .move(isShuffle)
  .none()
```

The duration is expressed in seconds. It will configure both the time before the next move is played, and the CSS animation duration.

In development mode, you can speed up the animations in the browser console: `game.setAnimationsSpeed(2)` (2x faster).

You can also slow the animations: `game.setAnimationsSpeed(0.5)` (2x slower)

:bulb: we might add an option later in the menu on to allows players to configure their own animation speed.

# Hide the cards

We offer a very convenient API to hide cards or other items from the players and spectators.

## Hidden or Secret

You must determine if your game has hidden or secret information.
* If everything is visible by everyone, you can skip this section.
* If some information is hidden but everyone can see the same thing, you have hidden information.
* If some players can see things that other players cannot (like a hand of card), then you have secret information.

For hidden information, open the [Rules file](https://github.com/gamepark/board-game-template/blob/main/rules/src/GameTemplateRules.ts) and extend `HiddenMaterialRules` instead of `MaterialRules`:

```typescript
import { HiddenMaterialRules } from '@gamepark/rules-api'
export class GameTemplateRules extends HiddenMaterialRules {
  // ...
}
```

For secret information, extends `SecretMaterialRule` instead:

```typescript
import { SecretMaterialRules } from '@gamepark/rules-api'
export class GameTemplateRules extends SecretMaterialRules {
  // ...
}
```

## Hiding strategies

Both hidden and secret rules requires `hidingStrategies`:

```typescript
export class GameTemplateRules extends SecretMaterialRules {
  hidingStrategies = {
    [MaterialType.Card]: {
      [LocationType.Deck]: hideItemId,
      [LocationType.PlayerHand]: hideIdToOthers,
    }
  }
}
```

The strategy will hide some information about the item of a specific type, in a specific location type. In this example, we hide the cards id when it is in the deck,
and we hide their id when in a player hand **except for the player matching the item's `location.player` field.**

:warning: Because of how these strategies work, it is important to use the `location.player` field to identify the owner of the item.

You will generally only need the hiding strategies available in the framework:
* `hideItemId` for HiddenMaterialRules and SecretMaterialRules
* `hideItemIdToOthers` only for SecretMaterialRules

:bulb: `hideFront` and `hideFrontToOthers` are also available if you need to [hide only one side of a card](TODO)

:confetti_ball: Once you have set the hiding strategies, **everything happen automatically**: you do not need to care about what to hide or reveal to players or spectators anywhere else in the code.
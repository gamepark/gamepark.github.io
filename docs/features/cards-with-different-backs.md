# Cards with different backs

In some games, cards with different images on their back side are mixed together.

When the cards are never mixed together, you should simply create different Material types.

However, if the cards are mixed, you cannot do that. Shuffling would be impossible, and every other operation would be much harder as you can only work on 1 material type at a time with the Material API.

For this reason, we use an object as the card id:

```typescript
export type CardId = {
  front?: SomeEnum
  back: SomeOtherEnum
}
```

Examples can be found in [Château Combo](https://github.com/gamepark/chateau-combo/blob/main/rules/src/material/Card.ts#L90), [Boreal](https://github.com/gamepark/boreal/blob/main/rules/src/material/Card.ts#L77), ...

:bulb: You can use anything as the item id, however the framework offers built-in tools if you use and object with `front` and `back`, so it is highly recommended to stick to that.

When your id is an object, the [Material description](https://github.com/gamepark/react-game/blob/main/src/components/material/FlatMaterial/FlatMaterial.tsx) will by default use the front property to get the front image key, and the back property for the back image key, like in [Château Combo](https://github.com/gamepark/chateau-combo/blob/main/app/src/material/ChateauComboCardDescription.tsx):

```typescript
export class ChateauComboCardDescription extends CardDescription {
  backImages = {
    [Place.Castle]: Castle,
    [Place.Village]: Village
  }

  images = {
    [Card.Steward]: Steward,
    [Card.HisHoliness]: HisHoliness,
    [Card.Chaplain]: Chaplain,
    //...
  }
}
```

Finally, you can very easily hide the card's front only using our built-in hiding strategies:

```typescript
export class GameTemplateRules extends SecretMaterialRules {
  hidingStrategies = {
    [MaterialType.Card]: {
      [LocationType.Deck]: hideFront,
      [LocationType.PlayerHand]: hideFrontToOthers
    }
  }
}
```

:bulb: The front can usually be hidden, that's why in the CardId type example the front field is typed as optional.

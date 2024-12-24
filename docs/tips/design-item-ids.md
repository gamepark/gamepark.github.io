# Design item ids

The items `id` field as `any` type. This freedom is helpful but can also lead to bad design decisions.

The first question to ask is: "do I need to partially hide the item id?". For instance, if [you need the card's front and back](features/cards-with-different-backs.md) inside the id, but you want to hide only the front, then you have to use an object as the id:

```typescript
export type CardId = {
  front?: SomeEnum
  back: SomeOtherEnum
}
```

Using an object here is mandatory because the [Hiding Strategy API](features/custom-hiding-strategies.md) can only hide paths inside the item.

:warning: **If you do not need to partially hide the item id, then you should use a number.**

The item id is saved inside the database and used a lot inside all the moves, all the time.

It is still possible to encode 2 information at once in the id. For example with a classic card game:

```typescript
enum Card {
  HeartsAce = 101,
  Hearts2 = 102,
  Hearts3 = 103,
  Hearts4 = 104,
  //...
  HearthKing = 113,
  DiamondsAce = 201,
  Diamonds2 = 202,
  //...
}

enum CardColor {
  Hearts = 1,
  Diamonds = 2,
  Club = 3,
  Spades = 4
}

export const getCardColor = (card: Card): CardColor => Math.floor(card / 100)
export const getCardValue = (card: Card) => card % 10
```

Using this design allow to encode 2 information inside an id of number type. The utility function making the bridge to get any information.

Do not overuse it to try to encode a lot of information, or if it makes some information harder to access.

If the card hold more information, simply use a mapping to a description object, like in [AracKhan Wars](https://github.com/gamepark/arackhan-wars/blob/main/rules/src/material/FactionCard.ts#L353) for instance.

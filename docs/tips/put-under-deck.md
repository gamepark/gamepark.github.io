# Put a card under a deck

When you have a deck of cards, you need to identify the order of the cards.

The standard way to do that is to use a [PositiveLocationStrategy](features/location-strategies.md#positive-location-strategy).

This way, the `location.x` property with go from x=0 to x=(number of item - 1).

By convention, the position x=0 is the position of the card at the bottom of the deck. The DeckLocator assumes that.

To put a card under a deck, simply move it to the x=0 position:

```typescript
card.moveItem({ type: LocationType.Deck, x: 0 })
```

The PositiveLocationStrategy will automatically increment all the other cards in the deck by 1.

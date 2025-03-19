# Items disappear under another item

The Game Table is a 3D environment (css `preserve-3d`). The relative position of the items is not handled with a css `z-index`, but using a css `translateZ`.

When an item disappear under another item, it is either due to:
- having a lower - or equal - translateZ
- being removed from the game table by the `Locator.hide()` function

Each type of locator behave differently, so we will review for each of them what could cause the troubles.

## ListLocator, DeckLocator, FlexLocator, HandLocator, PileLocator

All those locators depends on **the index of the item in the list**, determined by the function `Locator.getItemIndex`.

By default, the item index is location.x, fallback if undefined to location.y, then location.z, then the item's display index (for items with quantities).

Based on the item index, those locators will add by default: `translateZ(itemIndex * 0.05em)`, to ensure items are stacking in the expected order.

Therefore, you need to check:
- That you have a [location strategy](features/location-strategies.md) to put the expected location.x on every item.
- That your items share the same [Location area](concepts/location-area.md), otherwise the new location area will generate its own sequence.
- If you use a basic Locator, you need to handle the translateZ yourself.

## DeckLocator, PileLocator

Those locators have a default display limit of 20 items. Only the last 20 items are displayed.
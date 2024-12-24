# Location strategy not working

If you use a [location strategy](features/location-strategies.md) and do not modify the items directly in the rules (items should only be modified by the item moves), then the location strategy is always applied.

If 2 cards go to the same position (for example, x=0 in a PositiveSequenceStrategy), the reason is most probably because **they do not have the same [location area](concepts/location-area.md)**.

For example, one item can have a `location.id` or a `location.player` while the other don't.

:warn: If the item locator on the front end does not care about the id or the player, the items will be displayed at the same spot, however they do not have the same location area in the data.

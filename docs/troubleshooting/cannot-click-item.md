# Cannot click on item

Items with a negative translateZ cannot be clicked on Firefox: they are under the table (on Chrome it works).

By default, the ListLocator and the HandLocator apply a z-gap of 0.05, which is very convenient to have items display on top of each other in the right order. However, if the item index is negative (for instance if location.x is negative), then the translateZ will be negative, thus causing the bug.

In this case, you need to override either gap.z, coordinates.z or getItemIndex in order to never have a negative translateZ.

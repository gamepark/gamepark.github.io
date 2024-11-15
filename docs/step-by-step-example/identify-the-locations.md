# Identify the locations

The material is located around the table according to the rules.

The types of locations are listed in [/rules/src/material/LocationType.ts](https://github.com/gamepark/colt-super-express/blob/main/rules/src/material/LocationType.ts)

Again, we follow the terms from the rules as much as we can.

Try to be clear and consistent when naming the location types, as it will be used everywhere in the code:

- Do not use the same name as in MaterialType: if something is placed "on a card", name the location type "OnCard", not just "Card"
- Most of the material is directly on the table: you can suffix the name with "Spot", "Zone" or "Area" (BoardSpot, etc.)
- Do not hesitate to rename stuff later on!
- It is easier not to reuse the same location type to locale different item type. For example, use "PlayerMarkerSpot" and "PlayerBoardSpot" instead of a single "PlayerArea".
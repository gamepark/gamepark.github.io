# Display a location

In some game you might want to display extra information that are not game items, such as areas or counters.

For such use cases, we use locations and location descriptions to display the location on the screen.

By default, locations only appears on-the-fly when you can drop on it an item that you are currently dragging.

## LocationDescription

The location description is the class that describe what a location looks like:

```typescript jsx
class SomeLocator extends Locator {
  //...
  locationDescription = new LocationDescription({ width: 5, height: 5, borderRadius: 1 })
}
```

`LocationDescription` is useful to create locations that do not act as drop areas. If you need to drop items on that location, use `DropAreaDescription` instead.

You can access more feature on the location descriptions by extending the class:

```typescript jsx
class SomeLocator extends Locator {
  //...
  locationDescription = new SomeLocationDescription({ width: 5, height: 5, borderRadius: 1 })
}

class SomeLocationDescription extends DropAreaDescription {
  extraCss = css`...`
  help = SomeHelpDialog
}
```

Many examples are available on Github: https://github.com/search?q=org%3Agamepark%20DropAreaDescription&type=code. You can use location descriptions to:
* display some areas on the table all the time (like in [Faraway](https://github.com/gamepark/faraway/blob/main/app/src/locators/description/PlayerRegionAreaDescription.tsx) or [Along History](https://github.com/gamepark/along-history/blob/main/app/src/locators/CivilisationAreaDescription.ts))
* display a counter on top of a deck (like in [Architects of Amytis](https://github.com/gamepark/architects-of-amytis/blob/main/app/src/locators/MainBoardStackSpaceLocator.ts))
* highlight a small area on a card during the tutorial (like in [Living Forest Duel](https://github.com/gamepark/living-forest-duel/blob/main/app/src/locators/AnimalCostLocator.ts))

## Static locations

The location description is not enough to have an actual location displayed. A `Location` object must also exist independently of an item.

* Inside a tutorial, you can focus a location. If the focused location does not exist, it is created on-the-fly.
* When you drag an item, every valid drop locations are also create on-the-fly if they do not exist.
* Otherwise, you need to create the locations.

If the location does not have a parent item, there is only one way to create it, inside the locator:

```typescript jsx
export class PlayerRegionLocator extends FlexLocator {
  //...
  getLocations(context: MaterialContext) {
    return [] // the list of the locations
  }
}
```

:bulb: The `type` attribute for the location is optional here: by default the location type matching that locator will be applied.
:bulb: If the locations does not depend on the context, you can use the `locations` or `location` fields as a shortcut.

If the location has a parent item, you can either **use the Locator** (example above) to describe the locations, or you can **use the parent item's material description**:

:warning: If you use the locator, the locations will exist on every item. Use `MaterialDescription.getLocations` (see below) to create the locations based on the item's argument.

Here is an example with the locations from the board in [Expedition](https://github.com/gamepark/expedition/blob/main/app/src/material/BoardDescription.tsx):
```typescript jsx
class ExpeditionBoardDescription extends BoardDescription {
  //...
  locations = nodes.map<Location>(place => ({ type: LocationType.Place, id: place }))
    .concat([
      { type: LocationType.Place, id: RedNode.CraterLake_NorthWest, x: 1 },
      { type: LocationType.Place, id: RedNode.Teotihuacan_SouthWest, x: 1 },
      { type: LocationType.Place, id: RedNode.RapaNui_South, x: 1 }
    ])
    .concat(roads.map(road => ({ type: LocationType.Road, id: road })))
}
```

:bulb: You can also use `getLocations` if the item's internal locations depends on the item or the context:

```typescript jsx
class SomeItemDescription extends BoardDescription {
  getLocations(item: MaterialItem, context: ItemContext) {
    return [] // locations depending on the item or the context
  }
}
```

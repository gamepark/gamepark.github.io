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

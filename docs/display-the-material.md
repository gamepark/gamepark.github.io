# Display the material

Once you have [created a few material items in the setup](game-setup.md#setupmaterial) and [started a new game](console-commands.md#new),
you can start to display the items in the application.

## I. Describe the material

Go to `/app/src/material/Material.tsx`. This is where each [type of material](types-of-material.md) is mapped to their description (What they look like).

Create the description for your first Material type, usually a [card or a token](cards-boards-and-tokens.md).

## II. Locate the material

Go to `/app/src/locators/Locators.tsx`. This is where each [type of location](types-of-locations.md) is mapped to their locators.
A Locator is a class that is responsible for placing items on the screen where their belong.

For you first test, you can create the most simple ItemLocator this way:

```ts
// file: VerySimpleLocator.tsx
export class VerySimpleLocator extends ItemLocator {
  position: { x: 0, y: 0 } // Item will be in the middle of the "table"
}

// file: Locators.tsx
export const Locators: Record<LocationType, ItemLocator<PlayerColor, MaterialType, LocationType>> = {
  [LocationType.VerySimple]: new VerySimpleLocator()
}
```

You can extend [DeckLocator](deck-locator.md), [HandLocator](hand-locator.md) or [PileLocator](pile-locator.md) to display the items the way you like.

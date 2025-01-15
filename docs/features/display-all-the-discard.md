# Display all the discard

If you have a pile of cards (for example a discard) and the players should be able to consult all the cards, you can proceed this way:

1. For a start, clicking any card of the pile should open the location help instead of the item help.
You can do that by overriding `displayHelp` in the material description:

```typescript jsx
export class YourCardDescription extends CardDescription {
  // ...

  displayHelp(item: MaterialItem, context: ItemContext) {
    if (item.location.type === LocationType.Discard) return displayLocationHelp(item.location)
    return super.displayHelp(item, context)
  }
}
```

2. You create a help dialog for the location description, like in [Chateau Combo](https://github.com/gamepark/chateau-combo/blob/main/app/src/locators/component/DiscardHelp.tsx)

Using CSS, you can create an organized list of all the cards, and make sure every card can be clicked if the user wants to display the help for a specific card.

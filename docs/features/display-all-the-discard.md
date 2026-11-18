# Display all the discard

If you have a pile of cards (for example a discard) and the players should be able to consult all the cards, you can proceed this way:

1. Clicking any card of the pile should open the location help instead of the item help. Override `displayHelp` in the material description:

```typescript jsx
export class YourCardDescription extends CardDescription {
  // ...

  displayHelp(item: MaterialItem, context: ItemContext) {
    if (item.location.type === LocationType.Discard) return displayLocationHelp(item.location)
    return super.displayHelp(item, context)
  }
}
```

2. Create the help dialog in the location description:

```typescript jsx
class DiscardDescription extends DropAreaDescription {
  help = DiscardHelp
}
```

3. Implement the help component, like [DiscardHelp in Château Combo](https://github.com/gamepark/chateau-combo/blob/main/app/src/locators/component/DiscardHelp.tsx). Using CSS, you can create an organized list of all the cards.

:warning: Make sure each card can be clicked to display its own help:

```typescript jsx
<MaterialComponent
  type={MaterialType.Card}
  itemId={card.id}
  css={pointerCursorCss}
  onClick={() => play(displayMaterialHelp(MaterialType.Card, card, index), { transient: true })}
/>
```

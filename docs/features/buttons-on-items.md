# Buttons on items

You can add buttons close to an item on the table if you need extra actions available on a simple click.

To do so, you have to implement `getItemMenu` in the MaterialDescription. [See examples](https://github.com/search?q=org%3Agamepark%20getItemMenu&type=code)

By default, the menu will only appear when the item is clicked, unless you add `menuAlwaysVisible = true` too.

If the menu is not always visible, it is highly recommended to include a help button to open the help dialog. The default help button can be built using `getHelpButton`.

Buttons are usually used to play a move:

```typescript jsx
export class PowerCardDescription extends CardDescription {
  getItemMenu(item: MaterialItem, context: ItemContext, legalMoves: MaterialMove[]) {
    const buy = moves.find(move => move.location.type === LocationType.BuyArea)
    return <>
      {this.getHelpButton(item, context, { angle: 20, radius: 5 })}
      {buy &&
        <ItemMenuButton label={<Trans defaults="buy"/>} angle={50} radius={4} move={buy}>
          <FontAwesomeIcon icon={faHandBackFist}/>
        </ItemMenuButton>
      }
    </>
  }
}
```

The `ItemMenuButton` component also accepts options:

```typescript jsx
<ItemMenuButton label="" angle={90} radius={3} move={card.rotateItem((rotation + 1) % 4)} options={{ transient: true }}>
  <FontAwesomeIcon icon={faRotateRight}/>
</ItemMenuButton>
```

Here, using `transient: true` means that the move is only played in the front-end, and will not be in the history (so it does not impact the undo).

Use `local: true` for a local move that goes inside the history.

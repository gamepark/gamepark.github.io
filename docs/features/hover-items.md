# Hover items

When the text of an item is small on the table, players should be able to read it by moving the pointer over it.

The [HandLocator](features/hand-of-cards.md) already does it: a hovered card comes on top of the others, straight, and twice as big. For other locators, override `getHoverTransform`:

```typescript
class RiverLocator extends ListLocator {
  getHoverTransform() {
    return ['translateZ(10em)', 'scale(2)']
  }
}
```

- It returns a list of CSS transforms to apply to the hovered item. On the table, 1em is 1 centimeter.
- Use `translateZ` to lift the item above its neighbours.
- The transform does not apply while the item is animated or dragged.
- A face-down item that has a back image does not zoom.

:warning: Touch screens have no hover: nothing essential should be available only this way. Players read the items in their [help dialogs](step-by-step-example/help-dialogs.md).

## Keep the zoom on screen

Close to the edges of the table, push the item back inside. Here, on the right edge:

```typescript
class RiverLocator extends ListLocator {
  getHoverTransform(item: MaterialItem, context: ItemContext) {
    const { x = 0 } = this.getItemCoordinates(item, context)
    const { width } = context.material[context.type]!.getSize(item.id)
    const scale = 2
    const overflow = x + width * scale / 2 - tableXMax
    return [`translate3d(${-Math.max(0, overflow)}em, 0, 10em)`, `scale(${scale})`]
  }
}
```

[Greylune](https://github.com/gamepark/greylune/blob/main/app/src/locators/CardHover.ts) shares such a function between its locators, for all the edges of the table.

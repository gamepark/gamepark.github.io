# Help dialogs

## Purpose

Some players like to master the rules of a game before playing, but many players just discover the game while playing it.

Help dialogs offer a contextual help when clicking on any item of the game.

The goal is to explain what is this material, and what is does in the game.

Sometime, we also create help dialogs for locations.

Like the [headers](step-by-step-example/write-the-headers.md), help dialog texts are translated. 

## Example

By default, the help dialog of a component only show the image of the component.

To display text on the right in the dialog, you have to create a React component and refer to that components inside the [Material Description](step-by-step-example/display-first-item.md):

```typescript
class FirstPlayerCardDescription extends CardDescription {
  // ...
  help = FirstPlayerCardHelp
}
```
```typescript jsx
export const FirstPlayerCardHelp = () => {
  const { t } = useTranslation()

  return <>
    <h2>{t('first-player-card')}</h2>
    <p>{t('end-game')}</p>
    <p>{t('scoring')}</p>
  </>
}
```

## Writing good help dialogs

Writing good help dialogs is hard and requires some skill in designing good User Experience (UX).

If you do not feel comfortable with it, we can write the texts in the translations file, so that you only have to integrate them.

Here are some guidelines:
- Show the material name in the label. You can personalize it, for example: "John's coins".
- For cards or tile with multiple information displayed, cover all the information displayed inside the texts.
- If there is so text on the material, **reproduce all the texts**. The card images will not be available in every language available on Game Park, and it is important that players can get a translation of the cards inside the popup.
- Display quantities when important: number of cards in the deck, number of coins in the pile, etc.

Have a look at other games help dialogs too!

## Buttons in Help Dialogs

You can display buttons in the help dialog to allow some moves to be played:
```typescript jsx
<p>
  <PlayMoveButton move={someMove} onPlay={closeDialog}>{t('move.discard')}</PlayMoveButton>
</p>
```

It is a good practice to be able to play the moves either by drag and drop or using clicks only.

The move is collected from the legal moves using `useLegalMove`, like in headers.

Have a look at [ChateauComboCardHelp](https://github.com/gamepark/chateau-combo/blob/main/app/src/material/help/ChateauComboCardHelp.tsx) for example.

## Help dialogs for locations

Sometime, instead of showing the help for the clicked item, you want to display the list of items inside the same location, for example when you want to display the list of cards inside a discard when you click on the top card.

To do that, the first thing is to redirect the click on the card to displaying the help for the location instead inside the [Material Description](https://github.com/gamepark/chateau-combo/blob/main/app/src/material/ChateauComboCardDescription.tsx):

```typescript jsx
export class ChateauComboCardDescription extends CardDescription {
  displayHelp(item: MaterialItem, context: ItemContext) {
    if (item.location.type === LocationType.Discard) return displayLocationHelp(item.location)
    return super.displayHelp(item, context)
  }
}
```

Then you need to create the help dialog for the location, inside the Location Description:

```typescript jsx
class DiscardDescription extends DropAreaDescription {
  help = DiscardHelp
}
```

Finally, you can implement a help component for the location that displays the list of items, like in [Château Combo DiscardHelp](https://github.com/gamepark/chateau-combo/blob/main/app/src/locators/component/DiscardHelp.tsx).

:warning: Make sure each item can be clicked to display the help for that item:

```typescript jsx
<MaterialComponent
  type={MaterialType.Card}
  itemId={card.id}
  css={pointerCursorCss}
  onClick={() => play(displayMaterialHelp(MaterialType.Card, card, index), { local: true })}
/>
```

## The navigation buttons

Help dialogs can have navigation arrows:

<img width="600" src="./_media/help-arrows.jpg"/>

By default, the navigation arrow with allow to navigate between all the items inside the same [**location area**](TODO). The default sorting with iterate though z, then y, then x.

You can override the default behavior like in [Boreal](https://github.com/gamepark/boreal/blob/main/app/src/locators/BoardCardLocator.ts#L22).

## Help dialogs for Rules

You can create Help Dialogs that are attached to a RuleId inside App.tsx like in [Along History](https://github.com/gamepark/along-history/blob/main/app/src/index.tsx#L22):

```typescript jsx
<GameProvider /* ... */ rulesHelp={RulesHelp}>
```

Then, open some help dialog about a Rule id using `displayRulesHelp`:
```typescript jsx
<Button onClick={() => play(displayRulesHelp(RuleId.Wars), { local: true })}>...</Button>
```
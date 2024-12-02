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

Have a look at the [advanced dialogs features](TODO) to learn how to create buttons, navigation arrows, links between help dialogs...

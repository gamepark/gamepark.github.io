# Write the headers

## Purpose of headers

The headers are the texts that appears on top of the screen. It must guide the user and tell him, at any point, who must do what.

## Internationalisation

Headers must be available in multiple language.

We use a [translation file](https://docs.google.com/spreadsheets/d/1qvN10Iaen2siq0s-m8tyB3U80Ksq5HiPKgPQuUOupwY/edit) for every texts in games that must be translated.

Once you have writing access, you can download the [translation.json](https://game-park.com/api/translations?game=code-of-the-game) file and override [the existing one in app/src](https://github.com/gamepark/board-game-template/blob/main/app/src/translations.json).

:warning: Do not forget to format the file after download, so that changes can be tracked!

## Headers guidelines

The headers must follow this rules, by order of priority:
1. If the player is awaited, tell him what he has to do
2. If only one player is awaited, display the player's name and what he has to do
3. If multiple players are awaited, simply say "Players must do..."

Also, try to never use the player's name when speaking about himself: do not say "John must do..." but "You must do...". Also try to say "Jane must take a card from you" instead of "Jane must take a card from John".

## Headers examples

Inside the [Headers.tsx](https://github.com/gamepark/board-game-template/blob/main/app/src/headers/Headers.tsx) file, each RuleId is mapped to a React Component responsible for displaying the header text for this rule.

Some headers are very simple, when the game phase is 100% automatic and not specific to one player. For example, in Chateau Combo [EndOfTurnHeader](https://github.com/gamepark/chateau-combo/blob/main/app/src/headers/EndOfTurnHeader.tsx):

```typescript jsx
export const EndOfTurnHeader = () => {
    const { t } = useTranslation()
    return <span>{t('move-messenger')}</span>
}
```

:bulb: We use [React-i18next](https://react.i18next.com/) to translate the texts

When only one player can be active in a rule, the header is [not much complex](https://github.com/gamepark/architects-of-amytis/blob/main/app/src/headers/ChooseBuildingTileHeader.tsx):

```typescript jsx
export const ChooseBuildingTileHeader = () => {
  const { t } = useTranslation()
  const rules = useRules<ArchitectsOfAmytisRules>()!
  const me = usePlayerId()
  const activePlayer = rules.getActivePlayer()
  const player = usePlayerName(activePlayer)
  if (activePlayer === me) {
    return <>{t('header.choose-building.you')}</>
  } else {
    return <>{t('header.choose-building.player', { player })}</>
  }
}
```

:bulb: `useRules`, `usePlayerId` and `usePlayerName` are custom [React hooks](https://react.dev/learn/reusing-logic-with-custom-hooks) available in the Game Park framework.

Sometimes, you need to have [custom moves and buttons in the header to play those moves](TODO).

Finally, headers for simultaneous game phases, like the [ExplorationHeader](https://github.com/gamepark/faraway/blob/main/app/src/headers/ExplorationHeader.tsx) from Faraway are more complex as they need to cover at least 3 cases.

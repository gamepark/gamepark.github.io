# Write the headers

## Purpose of headers

The headers are the texts that appears on top of the screen. It must guide the user and tell him, at any point, who must do what.

## Internationalisation

Headers must be available in multiple languages.

Translation files are stored in the repository at `app/public/translation/{locale}.json` (e.g. `en.json`, `fr.json`). Each file is a flat key-value JSON object:

```json
{
  "header.choose-building.you": "Choose a building tile",
  "header.choose-building.player": "{player} must choose a building tile"
}
```

:bulb: During development, you can write translations only in your native language. Translations to other languages will be done later. Add `?locale=fr` (or `es`, `en`...) to the URL to test the texts in your language. Missing keys are logged as warnings in development and as errors in production.

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

:bulb: We use [React-i18next](https://react.i18next.com/) with [ICU MessageFormat](https://unicode-org.github.io/icu/userguide/format_parse/messages/) to translate the texts.

:warning: Use **single braces** for interpolation: `{player}`, `{amount}`. Do **not** use double braces (`{{player}}`), which is the default i18next syntax but not used here.

When only one player can be active in a rule, the header is [not much complex](https://github.com/gamepark/architects-of-amytis/blob/main/app/src/headers/ChooseBuildingTileHeader.tsx):

:warning: never use a legal moves as a basis for display conditions in headers: the header will break in the tutorial (when legal moves are filtered) or during animations as well.

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

Sometimes, you need to have buttons in you headers, for example to play [custom moves](features/custom-moves.md):

```typescript jsx
export const PlayerTurnHeader = () => {
  const pass = useLegalMove(isCustomMoveType(CustomMoveType.Pass))
  return (
    <Trans defaults="header.turn.me" components={{
      pass: <PlayMoveButton move={pass}/>
    }}/>
  )
}
```

Finally, headers for simultaneous game phases, like the [ExplorationHeader](https://github.com/gamepark/faraway/blob/main/app/src/headers/ExplorationHeader.tsx) from Faraway are more complex as they need to cover at least 3 cases.

# Game Options

The game options are all the configurations that must be chosen before the game starts.

It always includes, al least, the number of players.

Game options will be displayed as configuration options in various screen on the website:

<img width="800" src="./_media/iaww-options.jpg" style="display: block"/>

<img width="800" src="./_media/iaww-matchmaking-options.jpg"/>

All the game options must be configured in the [options file](https://github.com/gamepark/board-game-template/blob/main/rules/src/GameTemplateOptions.ts).

The game options are available to use in the game setup:

```typescript
export class GameTemplateSetup extends MaterialGameSetup {
  setupMaterial(options: GameTemplateOptions) {
    // use the options here
  }
}
```

## Players identifiers

The most common option is the player identifier. If players can pick a color (or something else) that will be their identity during the game, then it can be an option.

If you do not have anything to identify the players, skip this section and go to [No player identifier](#no-player-identifier)

It is configured this way by default in the template:

```typescript
type PlayerOptions = { id: PlayerColor }

export type GameTemplateOptions = {
  players: PlayerOptions[]
}

export const GameTemplateOptionsSpec: OptionsSpec<GameTemplateOptions> = {
  players: {
    id: {
      label: (t: TFunction) => t('Player color'),
      values: playerColors,
      valueSpec: color => ({ label: t => getPlayerName(color, t) })
    }
  }
}

export function getPlayerName(playerId: PlayerColor, t: TFunction) {
  switch (playerId) {
    case PlayerColor.Red:
      return t('Red')
    case PlayerColor.Blue:
      return t('Blue')
    case PlayerColor.Green:
      return t('Green')
    case PlayerColor.Yellow:
      return t('Yellow')
  }
}
```

The option and values labels will be displayed on the website. The translation keys (here "Player color", "Red", "Blue", "Green" and "Yellow") must be in the [translation file](https://docs.google.com/spreadsheets/d/1qvN10Iaen2siq0s-m8tyB3U80Ksq5HiPKgPQuUOupwY/edit?usp=sharing), and the "Website" column must be checked.

:bulb: the "Game" column must also be checked for the player id option values, because they will be used as the fallback name for player's without a name.

:warning: With player id options, `this.game.players` will contain unique identifiers for the players in a random order.

**Finally, you need to include images for the player id choice in Friendly games.** For each valid identified, you need to add an image inside app/src/public: `player-id-1.png`, `player-id-2.png`, etc.

Player choice images can have transparency, and must be 128 x 72 pixels.

## No Player Identifier

If the game as nothing to identify the players except their seat around the table, the options must be simplified this way:

```typescript
export type GameTemplateOptions = {
  players: number
}

export const GameTemplateOptionsSpec: OptionsSpec<GameTemplateOptions> = {}
```

Every occurrence of `PlayerColor` in the code can be deleted and replaced with type `number`.

:warning: Without player id options, `this.game.players` will contain this kind of sequence, ordered: `[1, 2, 3, 4...]`. Players are always given random seats.

## Boolean options

Boolean options are options that can be answered by yes or no: "Do you want to play with the expansion?", "Do you want to play with this variant?", etc.

Have a look at [AracKhan Wars options](https://github.com/gamepark/arackhan-wars/blob/main/rules/src/ArackhanWarsOptions.ts):

```typescript
export type ArackhanWarsOptions = {
  deckbuilding: boolean
}

export const ArackhanWarsOptionsSpec: OptionsSpec<ArackhanWarsOptions> = {
  deckbuilding: {
    label: (t: TFunction) => t('deckbuilding'),
    help: (t: TFunction) => t('deckbuilding.help')
  }
}
```

:bulb: the label and help texts will be displayed on the website in the Friendly and Competitive games configurations.

## Enum options

Enum options are options with multiple values.

For example, in [Captain Flip](https://github.com/gamepark/captain-flip/blob/main/rules/src/CaptainFlipOptions.ts) you can choose which board all players will play with.

Each option must have a label, and can also have a help text.

More examples of Enum options are available with [Architects of Amytis](https://github.com/gamepark/architects-of-amytis/blob/main/rules/src/ArchitectsOfAmytisOptions.ts) or [Arctic](https://github.com/gamepark/arctic/blob/main/rules/src/ArcticOptions.ts) (6 options, side A or B for 6 cards).

:bulb: you do not need to provide images for those options like for player ids.

## Options validation

For example, in It's a Wonderful World, you need the Corruption & Ascension expansion to play with 6 or 7 players, or with Empire cards side E or F.

To enforce constraints between multiple options, you can implement the `validate` field:

```typescript
export const ItsAWonderfulWorldOptionsSpec: OptionsSpec<ItsAWonderfulWorldOptions> = {
  corruptionAndAscension: {
    label: t => t('Corruption & Ascension'),
    help: t => t('c&a.help'),
    subscriberRequired: true
  },
  players: {
    id: {
      label: t => t('Empire'),
      values: empireNames,
      valueSpec: empire => ({
        label: t => getPlayerName(empire, t)
      })
    }
  },
  empiresSide: {
    label: t => t('Empire cards side'),
    values: empireSides,
    valueSpec: side => ({
      label: t => t('Side {side}', {side: String.fromCharCode(64 + side)}),
      help: t => getEmpireSideHelp(side, t),
      warn: t => side === EmpireSide.A ? t('Side A is advised for beginners') : '',
      subscriberRequired: side !== EmpireSide.A && side !== EmpireSide.B
    })
  },
  validate: (options, t) => {
    if (options.corruptionAndAscension === false) {
      if (options.players && options.players.length > 5) {
        throw new OptionsValidationError(t('6.players.requires.c&a'), ['corruptionAndAscension', 'players'])
      }
      if (options.empiresSide === EmpireSide.E || options.empiresSide === EmpireSide.F) {
        throw new OptionsValidationError(t('face.e.f.requires.c&a'), ['corruptionAndAscension', 'empiresSide'])
      }
    }
  }
}
```

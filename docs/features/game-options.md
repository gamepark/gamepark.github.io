# Game Options

The game options are all the configurations that must be chosen before the game starts.

It always includes, at least, the number of players.

Game options will be displayed as configuration options in various screens on the website:

<img width="800" src="./_media/iaww-options.jpg" style="display: block"/>

<img width="800" src="./_media/iaww-matchmaking-options.jpg"/>

All the game options must be declared in the [options file](https://github.com/gamepark/board-game-template/blob/main/rules/src/GameTemplateOptions.ts).

## The declaration is structure only

Since framework **v7.7**, a game declares its options with `OptionsSpecV2`: a plain JSON object, with no
functions and no text.

```typescript
import { getEnumValues, OptionsSpecV2 } from '@gamepark/rules-api'
import { PlayerColor } from './PlayerColor'

export const GameTemplateOptionsSpecV2: OptionsSpecV2 = {
  specVersion: 2,
  players: { min: 2, max: 4 },
  identities: { values: getEnumValues(PlayerColor) }
}
```

Being plain JSON is the point: the platform copies the declaration into its database when your bundle is
prepared, so every screen that needs to know what your game can be configured with reads it from there
instead of downloading and running your game.

Three things a game used to declare are deliberately *not* in it:

| What | Where it lives now |
|------|--------------------|
| Labels and help texts | Your game's options document, `app/public/options/{locale}.json` |
| Subscription and competitive gates | The platform database, edited by Game Park |
| `validate` | Expressed as rules the platform can read — see [Constraints](#constraints) |

`players` is the range of legal table sizes, and it is the root that every other range narrows. It must
match the player count declared for your game on the platform: a disagreement silently changes which
tables exist.

The object your setup receives is unchanged:

```typescript
export class GameTemplateSetup extends MaterialGameSetup {
  setupMaterial(options: GameTemplateOptions) {
    // use the options here
  }
}
```

## Where the texts go

Publish one file per locale in `app/public/options/{locale}.json`, beside the translations your game
already serves. It is a flat key-value object, and the keys follow a convention derived from the spec:

| Key | What it names |
|-----|---------------|
| `option.<option>` | The option itself |
| `option.<option>.<value>` | One value of an `enum` or `enum-set` option |
| `identities` / `teams` | The identity or team concept |
| `identities.<value>` / `teams.<value>` | One identity or one team |
| `<any key above>.help` | An explanation shown under the label |
| `<any key above>.warn` | A warning shown when that value is picked |

Only the labels are required. `help` and `warn` are optional by nature.

```json
{
  "identities": "Color",
  "identities.1": "Blue",
  "identities.2": "Red",
  "option.expansion": "Expansion",
  "option.expansion.help": "Adds the six advanced modules to the base game."
}
```

Values are addressed by their value, not by their name in your enum: `PlayerColor.Blue = 1` gives
`identities.1`. Keep that in mind if you ever renumber an enum — the keys follow the numbers.

The files are deployed with the rest of `app/public/` by `yarn deploy`.

:bulb: These labels are also what names a player who has no name — a bot, or a player who is not signed
in. `usePlayerName` reads `identities.<value>` from this document.

## Player identities

The most common option is the player identity: players pick a color, a faction, an empire, and that
choice identifies them for the whole game.

```typescript
type PlayerOptions = { id: PlayerColor }

export type GameTemplateOptions = {
  players: PlayerOptions[]
}

export const GameTemplateOptionsSpecV2: OptionsSpecV2 = {
  specVersion: 2,
  players: { min: 2, max: 4 },
  identities: { values: getEnumValues(PlayerColor) }
}
```

An identity is unique by construction: it is what links a user to a player, so two players can never hold
the same one.

A value can be restricted, or made mandatory:

```typescript
identities: {
  values: [
    { value: 1, playerCount: { min: 3 }, required: true }, // the Prince only exists from 3 players, and someone must take it
    2, 3, 4, 5, 6
  ]
}
```

:warning: With identities, `this.game.players` contains those identifiers in a random order.

**You also need one image per identity**, used in the friendly game creation form. Put them in
`app/public`, named `player-id-<value>.png` — `player-id-1.png`, `player-id-2.png`, etc. They may have
transparency and must be 128 × 72 pixels.

### No player identity

If nothing identifies the players except their seat around the table, declare no identities:

```typescript
export type GameTemplateOptions = {
  players: number
}

export const GameTemplateOptionsSpecV2: OptionsSpecV2 = {
  specVersion: 2,
  players: { min: 2, max: 4 }
}
```

Every occurrence of `PlayerColor` in the code can be deleted and replaced with type `number`.

:warning: Without identities, `this.game.players` contains an ordered sequence: `[1, 2, 3, 4...]`.
Players are always given random seats.

## Teams

Teams are the only structural constraint on the table size itself: a table that cannot be split into
equal teams is not a legal table.

```typescript
export const KitsuOptionsSpecV2: OptionsSpecV2 = {
  specVersion: 2,
  players: { min: 2, max: 6 },
  teams: { values: teamColors }
}
```

The platform derives the legal player counts from the teams — two clans of equal size means 2, 4 or 6
players, never an odd count — and assigns the teams balanced. Neither rule is yours to restate. Add
`size` when a team has a fixed number of players instead of an equal split.

## Boolean options

An option answered by yes or no: "play with the expansion?", "play with this variant?".

```typescript
export type ArackhanWarsOptions = {
  deckbuilding: boolean
}

export const ArackhanWarsOptionsSpecV2: OptionsSpecV2 = {
  specVersion: 2,
  players: { min: 2, max: 2 },
  options: {
    deckbuilding: { kind: 'boolean' }
  }
}
```

A boolean is a single question with no value labels, so it needs only `option.deckbuilding` (and
optionally `option.deckbuilding.help`) in your options document.

Declare `values` only to constrain one side of it — see [Constraints](#constraints).

## Enum options

An option with several values.

```typescript
export const CaptainFlipOptionsSpecV2: OptionsSpecV2 = {
  specVersion: 2,
  players: { min: 2, max: 5 },
  options: {
    board: { kind: 'enum', values: boardTypes },
    treasureMap: { kind: 'enum', values: treasureMapTypes }
  }
}
```

Reference your own enum values rather than writing numbers: `values: boardTypes` keeps the declaration
readable and moves with the enum.

More examples with [Architects of Amytis](https://github.com/gamepark/architects-of-amytis/blob/main/rules/src/ArchitectsOfAmytisOptions.ts)
or [Arctic](https://github.com/gamepark/arctic/blob/main/rules/src/ArcticOptions.ts) (6 options, side A or B for 6 cards).

:bulb: unlike identities, enum options need no images.

## Enum-set options

An option where the host picks *several* values out of a list — for example four mythologies out of
eight.

```typescript
export const MythologiesOptionsSpecV2: OptionsSpecV2 = {
  specVersion: 2,
  players: { min: 2, max: 4 },
  identities: { values: getEnumValues(PlayerColor) },
  options: {
    mythologies: { kind: 'enum-set', values: getEnumValues(Mythology), size: 4 }
  }
}
```

`size` is how many values are picked. A number is shorthand for `{ min: n, max: n }`.

On the website the host does not have to pick all four: they can include some, exclude others, and leave
the rest to chance.

## Constraints

`validate` is gone. A body of code the platform cannot read forces it to guess and check; a declaration
it can read lets it *search*, so an impossible combination comes back as the rule that forbids it instead
of as an exception thrown from inside your game.

Three forms cover what `validate` used to express.

### A value that only exists at some table sizes

```typescript
options: {
  soloDifficulty: { kind: 'enum', playerCount: { max: 1 }, values: difficulties }
}
```

`playerCount` sits on the option to remove the whole question, or on a single value to remove just that
one:

```typescript
corruptionAndAscension: {
  kind: 'boolean',
  values: [{ value: false, playerCount: { max: 5 } }, true] // above 5 players, it cannot be turned off
}
```

### A value that depends on another option

```typescript
empiresSide: {
  kind: 'enum',
  values: [1, 2, 3, 4, { value: 5, requires: [{ option: 'corruptionAndAscension', values: [true] }] }]
}
```

`requires` holds when the named option has one of the given values. All the terms must hold.

### A combination the game refuses

```typescript
rules: [
  {
    type: 'forbidden-combination',
    when: [
      { option: 'treasureMap', values: [TreasureMapType.Base] },
      { option: 'board', values: boardsForbiddenWithBaseTreasureMap }
    ],
    message: 'base-map.forbidden'
  }
]
```

All the terms matching at once is what is forbidden — so "at least one of three decks" reads as "not all
three false".

`message` is a key in your options document, never text. It is the same key `OptionsValidationError`
carried under v1, so your wording survives the move word for word:

```json
{ "base-map.forbidden": "The Base treasure map cannot be used with these boards" }
```

Prefer the narrowest form that says what you mean. `requires` on a value and a `forbidden-combination`
can often express the same set of legal pairs; pick the one that names the rule once rather than
repeating it on every value.

## What the platform owns

These were declared in the game under v1. They are now held in the platform database, so Game Park can
change them without you releasing your game again:

- **`subscriberRequired`** — an option, or one of its values, reserved for subscribers
- **`competitiveDisabled`** — an option not offered in ranked play
- **`competitiveValue`** — the value ranked games are always played with
- **`competitivePlayers`** — the table sizes ranked play uses
- **default values** for friendly games

Tell Game Park what your game needs; do not try to express it in the spec.

## Migrating from v1

1. Replace `OptionsSpec<YourOptions>` with `OptionsSpecV2` and add `specVersion: 2` and `players`.
2. Turn each option into `{ kind: 'boolean' | 'enum' | 'enum-set', values }`. Keep referencing your enums.
3. Move `players.id` to `identities`, `players.team` to `teams`.
4. Move every `label`, `help` and `warn` text into `app/public/options/{locale}.json` under the
   convention keys above.
5. Rewrite `validate` as `playerCount`, `requires` or `forbidden-combination`, keeping the i18n keys your
   `OptionsValidationError` used.
6. Drop `subscriberRequired`, `competitiveDisabled` and `competitivePlayers`, and tell Game Park.
7. Export the new constant from `rules/src/index.ts` and pass it to `<GameProvider optionsSpec={…}>`.
8. Require `"@gamepark/rules-api": ">=7.7"` in `rules/package.json`.

The whole catalogue has been migrated, so any released game is an example.

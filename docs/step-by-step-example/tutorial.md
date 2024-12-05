# Tutorial

The tutorial is the very last step of adapting a game. It should only be coded once the table layout is validated, because the tutorial focus and popup positions is highly dependent on it.

## Writing the texts

As for headers and help dialogs, tutorial texts are translated inside the translation file.

Good tutorial are very hard to write: we can write the texts so the developed only have to integrate them in the code.

Here are some guidelines:

- The user should play the game while learning the rules.
- Try to minimize the quantity of texts between 2 moves.
- Explain the game flow, not all the details. Details should be available in the help dialogs all the time.
- The PDF of the rules is often good at explaining the rules. Do not hesitate to copy-paste from it.
- Use the same terms as in the PDF of the rules.
- Try to cover all the features of the game in a minimum number of scripted moves.
- You can create custom locations to focus specific parts of the cards for example.

## The tutorial class

The tutorial class must be called inside [app/src/index.tsx](https://github.com/gamepark/architects-of-amytis/blob/main/app/src/index.tsx):

```typescript jsx
    <GameProvider {/* ... */}
      tutorial={new Tutorial()}>
      <App/>
    </GameProvider>
```

The [Tutorial class](https://github.com/gamepark/architects-of-amytis/blob/main/app/src/tutorial/Tutorial.tsx) is responsible for:
- the tutorial version
- the tutorial game options
- refers to the tutorial setup
- the opponents' names and avatars
- the description for all the steps of the tutorial

## Tutorial version

The tutorial state is stored inside the player's local storage. If some changes inside the rules, or the tutorial, make this state incompatible, it can lead to all sorts of bugs.

Increasing the tutorial version forces all those states do be erased (tutorial are restarted from scratch). Only useful after the game is deployed.

## Tutorial game options

The [game options](features/game-options.md) that will be passed to the tutorial setup.

## Tutorial setup

In most games, we need a custom setup for the tutorial to script the beginning of the game. For instance, to give specific cards to the players in their hands, etc.

The tutorial setup is a class that overrides the regular setup, to produce a semi-random setup.

It can be tricky to produce the right setup. For example, if you want to put 5 specific cards on top of a deck, it is recommended to:
1. Search and move the 5 cards to another location
2. Shuffle the deck
3. Move the 5 cards back in the deck

Many setup examples are available on GitHub:
- [Architects of Amytis](https://github.com/gamepark/architects-of-amytis/blob/main/app/src/tutorial/TutorialSetup.ts)
- [Château Combo](https://github.com/gamepark/chateau-combo/blob/main/app/src/tutorial/TutorialSetup.ts)
- [AracKhan Wars](https://github.com/gamepark/arackhan-wars/blob/main/app/src/tutorial/TutorialSetup.ts)
- [Living Forest](https://github.com/gamepark/living-forest/blob/main/app/src/tutorial/TutorialSetup.ts)
- [Faraway](https://github.com/gamepark/faraway/blob/main/app/src/tutorial/TutorialSetup.ts)
- [Captain Flip](https://github.com/gamepark/captain-flip/blob/main/app/src/tutorial/TutorialSetup.ts)
- ...

## Opponent's names and Avatars

Here is an example from [Captain Flip](https://github.com/gamepark/captain-flip/blob/main/app/src/tutorial/Tutorial.tsx):

```typescript jsx
export class Tutorial {
  // ...
  players = [
    { id: me },
    {
      id: opponent,
      name: 'Blackbeard',
      avatar: {
        topType: TopType.Eyepatch,
        facialHairType: FacialHairType.BeardMajestic,
        facialHairColor: HairColorName.Black,
        clotheType: ClotheType.GraphicShirt,
        clotheColor: ClotheColorName.Black,
        graphicType: GraphicType.Skull,
        eyeType: EyeType.Default,
        eyebrowType: EyebrowType.AngryNatural,
        mouthType: MouthType.Smile,
        skinColor: SkinColor.Light
      }
    }
  ]
}
```

:bulb: Avatars comes from [https://getavataaars.com/](https://getavataaars.com/): you can configure it here then reproduce the configuration in the code!

## Tutorial steps

Each tutorial step can have:
- A popup
- A focus
- A move to play

After the last step, the players is free to complete the game. The opponents play random moves, like the debug bot, unless a [Tutorial AI](TODO) is provided.

### Tutorial Popup

All you need is to provide the translated text, using the same translation file as for header and help dialogs.

By default, the popup is centered in the screen. You can use the position attribute to translate it away from the default position:
```typescript jsx
export class Tutorial {
  // ...
  steps: TutorialStep[] = [{
    popup: {
      text: () => <Trans defaults="tuto.welcome" components={{ 
        bold: <strong/>
      }}/>
    },
    position: { x: 10, y: -10 }
  },
  // ...
  ]
}
```

### Tutorial focus

In the tutorial, you can focus some items or some locations on the screen:

```typescript jsx
export class Tutorial {
  // ...
  steps: TutorialStep[] = [{
    // ... (popup)
    focus: (game: MaterialGame) => ({
      staticItems: {
        [MaterialType.BuildingCard]: [scoreBoardDescription.staticItem]
      },
      materials: [
        this.material(game, MaterialType.Architect).id(me),
        this.material(game, MaterialType.Pawn).id(me)
      ],
      locations: [{ type: LocationType.PlayerBoardStackSpace, player: me, x: 1, y: 1 }],
      margin: { left: 1, top: 1, right: 15, bottom: 15 },
      scale: 1.5
    })
  },
    // ...
  ]
}
```
You can focus either `staticItems` to target [items that are static](/step-by-step-example/display-first-item?id=display-a-static-item), `materials` to target [non-static items](/step-by-step-example/create-items), or `locations`.

If you target a location that usually does not exist on the table, it will be automatically created.

If you target a location that is a part of a parent item, a mask will be applied on top of the item to highlight the location:

<img width="600" src="./_media/focus-example.jpg"/>

By default, the focus will zoom on the table until only focused items and locations are visible. To add extra margin around those elements, or to prevent the zoom to be too high, you can use `margin` and/or `scale`.

:bulb: As a good practice, never use a focus without a popup to explain what is focused.

### Tutorial moves

When you want the player to play a move, just add a move section in the tutorial step:
```typescript jsx
export class Tutorial {
  // ...
  steps: TutorialStep[] = [{
    // ... popup & optional focus
    move: {}
  },
    // ...
  ]
}
```

:bulb: As a good practice, always combine popup + player move in the same step. This way, the player will be able to play the move without closing the popup. 

By default, the move will be the player's, add all the usual legal moves at this game step will be allowed. Of course, you usually want to restrict the legal moves, so that the player only has one option:

```typescript jsx
move: {
  filter: (move, game) => isMoveItemType(MaterialType.Card)(move) && this.material(game, MaterialType.Card).getItem(move.itemIndex).id === someCardId  
}
```

For opponents moves, you need to specify the player playing the move as well:

```typescript jsx
move: {
  player: opponentId
  // filter: ...  
}
```

:bulb: As a good practice, never use a popup steps with opponents moves. Otherwise, it will appear and disappear too quickly to be read.

Finally, sometimes you might need to interrupt a flow of consequences to explain what is happening step by step.

You can interrupt a consequence (and the following consequences) this way:

```typescript jsx
move: {
  // filter: ...  
  interrupt: (move) => isEndPlayerTurn(move) && move.player === opponent // for example
}
```

The interrupted consequences will be memorized, so that you can play them later on, after explaining what needs to be explained.

To restart the flow of consequences, simply add an empty move in some next step:

```typescript jsx
popup: {/*...*/}
move: {} // when player clicks "OK" in the popup, consequences will go on.
```

:bulb: You can interrupt the flow of consequences multiple times if you need to.

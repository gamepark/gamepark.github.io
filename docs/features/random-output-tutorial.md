# Random output in tutorials

When you create a tutorial, you want to control a part of what is usually random in the game.

For the setup part, using a custom [tutorial setup](step-by-step-example/tutorial.md#tutorial-setup) allows to prepare specific cards at specific locations.

After the setup, you sometimes need to control a random output anyway (such as dice for example).

In that case, you can override the randomize function inside the tutorial step configuration, like in [King of Tokyo Duel](https://github.com/gamepark/king-of-tokyo-duel/blob/main/app/src/tutorial/Tutorial.tsx):

```typescript jsx
export class Tutorial extends MaterialTutorial<Monster, MaterialType, LocationType> {
  steps: TutorialStep[] = [
    {
      popup: {
        text: () => <Trans defaults="tuto.roll" components={{ bold: <strong/> }}/>,
        position: { x: 30 }
      },
      focus: (game: MaterialGame) => ({
        materials: [this.material(game, MaterialType.Dice).player(me)],
        scale: 0.5
      }),
      move: {
        randomize: (move: MaterialMoveRandomized) => {
          if (isRoll(move)) {
            switch (move.itemIndex) {
              case 0:
                move.location.rotation = DiceFace.Claw
                break
              case 1:
                move.location.rotation = DiceFace.Heal
                break
              case 2:
                move.location.rotation = DiceFace.Energy
                break
              case 3:
                move.location.rotation = DiceFace.Energy
                break
              case 4:
                move.location.rotation = DiceFace.Fame
                break
            }
          }
        }
      }
    }
  ]
}
```

The randomize function takes a move already randomized in parameter and allows you to override the randomization with the custom value you need for the tutorial at this step.

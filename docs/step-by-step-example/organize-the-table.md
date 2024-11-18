# Organize the Table

## Screen size

We want our games to be available not only on desktop computers, but also on mobile devices.

However, we choose not to code 2 different user interface which would take a lot more time.

The material must be organized inside a rectangular "Table". The limits of the table is visible as a white border in dev mode:

<img width="600" src="./_media/empty-table.jpg"/>

If the screen is larger, extra lost space is on the left and right of the screen. Otherwise, extra space in on the bottom of the screen.

For mobile devices, we automatically ask users to flip their screen in order to play.

Therefore, **the layout of the game must be organized to fit in a landscape rectangle** (somewhere around the ratio 180/100).

:warning: Everything outside the white rectangle can be truncated for some users, so do not use that space!

You can change the table size in [GameDisplay.tsx](https://github.com/gamepark/colt-super-express/blob/main/app/src/GameDisplay.tsx):

```typescript jsx
<GameTable xMin={-50} xMax={50} yMin={-30} yMax={30}/>
```

The table does not have to be centered around the coordinates (x=0, y=0).

Also, the boundaries can depend on the number of players, and also dynamically grow during the game!

However, the bigger the table is, the smaller the components will be without zooming in.

## Player panels

Players panels indicates the player name, avatar and timer. It can be used to display more data, such as a score.

<img width="300" src="./_media/player-panel.jpg"/>

**Player panels are fixed**: they are not inside the table, so they will stay at the same position when you zoom in, etc.

However, player panels can be positioned anywhere on the screen, and it is advised to place the material owned by players close to their panels
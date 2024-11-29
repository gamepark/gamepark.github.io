# Identify the material

A board game is all about Boards, Cards, Tokens... moving around the table.

The types of material are listed in [/rules/src/material/MaterialType.ts](https://github.com/gamepark/board-game-template/blob/main/rules/src/material/MaterialType.ts)

We follow the terms from the rules as much as possible.

Here is an example from [Architects of Amytis](https://github.com/gamepark/architects-of-amytis):

<img width="800" src="./_media/architects-amytis-material-1.jpg" style="display: block"/>

<img width="800" src="./_media/architects-amytis-material-2.jpg"/>

```typescript
export enum MaterialType {
  MainBoard = 1,
  FavorBoard,
  ScoreBoard,
  PlayerBoard,
  Architect,
  Pawn,
  FirstPlayerCard,
  BuildingCard,
  ProjectCard,
  BuildingTile
}
```
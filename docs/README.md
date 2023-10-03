## Get started

Welcome to you, (future) developer!

If you love board games, and you would like to adapt one online, you have come to the right place :hugs:

## How it works

### Rules & App

On Game Park, **the rules are enforced**. It means that players can only do what is allowed by the rules.

To enforce the rules, the Game Park server must know them.

For that reason, each game is divided into 2 subprojects: `/app` and `/rules`

The `/rules` part will be embedded on Game Park server. It contains only the rules, nothing about how the game is displayed.

The `/app` part knows the `/rules` part and will compile into a React application, to display the game.

### State & Moves

Each game has a **state**. The state is the "photo" of the game, it is the data you need to continue the game when it has been stopped.

Each game has **moves**. The moves are the choices players can make during the game. Moves can also be automatic, when players have no choices but to follow the rules.

In the first version of Game Park API, you had to design the state and the moves of each game from scratch. This was complex and time-consuming, so we improved the framework!

We introduced the concepts of [Material & Locations](#Material-&-Locations). Now each game state has [the same Data structure](#Data-of-a-Material-Game) and [the same moves](#Moves-of-a-Material-Game).

### Material & Locations

Most board games consist of the rules plus a list of components: boards, cards, tokens, dices...: the **material** of the game

During the game, the material items move around the table and the players. At any time during the game, each item has a **location**.

**99% of a game state is about knowing what item we have, and at what location.**

### Rules

Most board game we adapt on Game Park have multiple sections in their rules: phases, rounds, steps, actions...

What players can do, or must do, in each part of the rules, is often very different.

For this reason, and because it simplifies the code a lot, you must split the rules into **rule parts** when you implement the game.

### Memory

In many game, there are some information that players must keep in mind for a short period of time (or sometime all the game).

Example: the starting player, the players' scores, how many actions I have left this turn...

This information, when playing the game for real, is either inside the player's head, or written on some paper (scoring sheet).

We need a special space to store this information on Game Park: we call it the **Memory**.

### Data of a Material Game

Here is the generic data structure of any game played on Game Park:
```ts
export type MaterialGame<Player extends number = number, MaterialType extends number = number, LocationType extends number = number> = {
  players: Player[]
  items: Partial<Record<MaterialType, MaterialItem<Player, LocationType>[]>>
  rule?: RuleStep<Player>
  memory: Record<keyof any, any>
}
```
* `players`: identifier of the players playing that game
* `items`: for each type of material that moves around during the game, we have the list of items
* `rules`: the current section of the game's rules that we are playing
* `memory`: any information that players have to memorize, often for a short period of time

### Moves of a Material Game

There are 4 kind of moves:
* ItemMove
* RuleMove
* CustomMove
* LocalMove
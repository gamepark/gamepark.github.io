# Hiding data

Most games does not offer [Complete information](https://en.wikipedia.org/wiki/Complete_information) to the players. Some information can be hidden because a deck of cards is shuffled, or players have cards in their hand.

Hiding data is an important feature for a board game framework that enforces the rules. Cards cannot simply be flipped on the client side: if the data is transmitted from the server to the client, then cheating is easy.

## Game view and move view

In the Game Park framework, there are 2 kind of information that the server can send to the client:
- the game state
- a game move

In the game state, information sometimes has to be hidden.

In the moves, sometime we need to hide some information, and sometimes we need to reveal information: for example, if a players draws a cards from a shuffled deck, that player also need to receive the id of the card!

The API has 2 functions: `getGameView` and `getMoveView`, to transform the game state and the move into what players can view of it.

## Incomplete or Secret information

Game Park relies on [Pusher](https://pusher.com/) for real time notifications: everytime a player plays a move, all the other players and spectators will receive a notification of the move played.

We could use, all the time, a custom channel to notify each player individually, and another channel to notify the spectator.

However, some games does not have "secret information": all the players always have the same information.

In that case, we can use a single notification channel to notify every player and spectators in the game (and save costs).

Therefore, there are 3 different level of information visibility on Game Park:
1. Complete information
2. Hidden information: some information is hidden, but the same information is available to everyone
3. Secret information: players have different information

In a game with secret information, the API has 2 move functions: `getPlayerGameView` and `getPlayerMoveView`.

## Hiding Material

With the material approach, hiding information has become much more simple. Using the [Hiding Strategies](features/custom-hiding-strategies.md), we are able to write automatically all the game views and game moves for everyone.

As long as only information about items has to be hidden and revealed, you do not have to implement `getGameView` and `getMoveView` by yourself.

However, there is still one issue think that needs to be checked: if players take card in their hand that use to be visible to other players, then play cards secretly from their hands, everytime a player takes a card in hand the hand has to be shuffled, [otherwise the cards played can be predicted](troubleshooting/predictable-cards.md)!

## Keeping moves secret

Some games have phases where players act simultaneously and their actions must remain hidden until everyone is done — for example, a secret planning phase.

Hiding Strategies hide **information about items** (like the front of a card), but they don't prevent **moves themselves** from being transmitted. An opponent would still see a card moving even if they can't see which card it is.

To delay the transmission of moves, implement `keepMoveSecret` in your rules class (which must extend `SecretMaterialRules`):

### `keepMoveSecret`

```
keepMoveSecret(move: MaterialMove, playerId: PlayerId): boolean
```

Return `true` to prevent this individual move from being sent to `playerId`. The framework re-evaluates this function whenever the game state changes. When it returns `false`, the move is revealed.

### Example: secret planning phase

```
keepMoveSecret(move: MaterialMove) {
  if (isEndPlayerTurn(move)) return false
  return this.game.rule?.id === RuleId.Planning
}
```

During the Planning rule, all moves are withheld from other players. `EndPlayerTurn` is excluded so that players can see who has finished planning. As soon as the rule changes (e.g. to Production), the function returns `false` and every hidden move is revealed at once.

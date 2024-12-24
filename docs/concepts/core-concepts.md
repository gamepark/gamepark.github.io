# Core concepts

The Game Park framework aims to achieve these goals:
1. Enforce the rules
2. Implement board games fast
3. Offer a high UI quality in each game
4. Long-lasting code

## Enforce the rules

### Architecture

We want to enforce the rules of each game. Players can only do what the rules allow them to do, and they can only see what the rules allow them to see.

Game Park servers enforces the rules through the Rules API. Each game must implement their rules following the Rules API, and the server is responsible for handling the workflow.

The API is designed following the [Domain-driven design](https://en.wikipedia.org/wiki/Domain-driven_design) principles.

Specifically, the rules of the game are:
- independent of the database
- independent of the communications between the server and the clients (players and spectators)

The API enforces that: you will never write a database query nor trigger a notification inside the rules of a game.

### Game state and moves

Another core principle followed by the framework is the [Event-driven architecture](https://en.wikipedia.org/wiki/Event-driven_architecture).

Specifically, on Game Park each move played during the game is saved in the database. The initial state of the game as well. Using the initial state and the moves played from the beginning, at any time we are able to reproduce the current state of the game.

Saving all the moves played has major benefits:
- We can replay any game easily
- Each game can automatically offer the undo feature

It also introduces a challenge: random outputs must be reproducible. We could use a [Random seed](https://en.wikipedia.org/wiki/Random_seed), however Javascript default random number generator does not enforce that. So we instead choose to save the random outputs inside the moves after they are generated.

## Implement board games fast

The first version of the Game Park framework relied on the principles exposed above, and it works just fine.

However, it did not guide anything regarding how to design the game state, the moves nor the UI.

10 games were implemented on that base, and it appeared that the same pattern had to be repeated on each game, and it was taking too much time to implement each game. It was also sometime too complex to implement for junior developers.

For those reasons, we added an extra layer on top of the framework: **the Material approach**.

The Material approach offers a standard data structure for the game state, the moves, as well as a default user interface for the games.

With this extra layer, game takes 5x less to implement:
- game state and moves are already implemented
- drag and drop is automatic
- animations are automatic
- undo is automatic
- hiding strategies makes securing the data almost automatic
- location strategies makes organising the items almost automatic
... and many other benefits.

Of course, it restricts a little bit what can be done in terms of UI, but the downside is minor and the original approach is still working: all the game implemented with the original framework works, because the core API did not change.

## User experience

To provide a great user experience, the client must know the rules of the game.

It is the only way it can:
- highlight the available actions
- prevent forbidden moves without having to rely on the server
- provide great contextual help
- execute the moves played immediately without waiting for the server in case of network latency

For this reason, we want to be able to run the same code on the server and the client.

As the client is a web browser (a choice made to have the largest possible audience), Javascript was the natural choice.

We use Typescript on top of Javascript because strong typing offers lots of benefits, especially to reduce the number of bugs in the code.

Each game should offer a tutorial as well as in game help dialogs to guide the users as much as possible.

Thanks to the Material approach, the items that can be moved when it is a player's turn are automatically highlighted.

## Long-lasting code

The rules of the games does not depend on anything but the API and the typescript language.

Each game UI is an independent React application that only depends on Game Park server GraphQL API.

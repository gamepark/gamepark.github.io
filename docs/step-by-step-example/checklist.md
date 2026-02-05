# Checklist

Once the project is complete, before it can hit production you need to check that all those tasks are done:

- [Winner is identified](step-by-step-example/end-of-the-game.md#identify-the-winner)
- All the headers are done
- All the help dialogs have useful text
- The tutorial is available
- Someone made a security review:
  - [hiding strategies](step-by-step-example/hide-the-cards.md) are properly setup
  - If players add cards in their hand that were visible, then play cards hidden, the hand is shuffled so that other players cannot predict what they play
- There are no TODOs or commented code left in the code
- The [player panels](step-by-step-example/organize-the-table.md?id=player-panels) are displayed with a nice background
- All the texts are available in English and French at least
- All texts have been checked by an automatic proofreader
- In app/public, the PDF of the rules is available in English and French
  - PDF must be cropped if needed and compressed
- In app/public, the favicon is generated using https://realfavicongenerator.net/
- In app/public, the cover, box and avatar images are available
- If the game options includes [players ids](features/game-options.md?id=players-identifiers), images for these are available in app/public
- The publisher tested and validated the game and the tutorial
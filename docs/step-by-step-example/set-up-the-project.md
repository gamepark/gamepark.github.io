# Set up the project

## Prepare the repository

:warning: **We usually take care of this step for you.**

Open [Game Park Board Game Template](https://github.com/gamepark/board-game-template), and click "Use this template" to "Create a new repository".

Then, checkout the repository on your computer using Git, and using your code editor, search and replace in every file:
* `Game Template` => `Name of your Game`
* `GameTemplate` => `NameOfYourGame`
* `game-template` => `name-of-your-game`

Also rename GameTemplateOptions.ts, GameTemplateSetup.ts and GameTemplateRules.ts in rules/src.

:bulb: `name-of-your-game` is the unique identifier for the game on Game Park. We provide it.

Finally, commit and push the changes using Git.

## Checkout the repository

Use [Git checkout](https://git-scm.com/docs/git-checkout/en) to check out the project repository on your computer, if we prepared the repository.

Then open the project in your code editor.

## Install the dependencies

We rely on **a lot** of external javascript libraries to run a game.

At the root directory of the project, run this command to install all the dependencies:

`yarn install`

:bulb: All the dependencies can be found in the `package.json` files, and will be installed in the `node_modules` folder.

:warning: Troubleshooting:
- FAT drives does not support symlink, which we need because we use yarn workspaces. You cannot run the project on a FAT drive.

## Start the game

Once the dependencies are install, you can run this command to start a debug session of the game:

`yarn dev`

It will start a local server on port 3000. Then open your browser at this url: http://localhost:3000/

:bulb: we use [Create React App](https://github.com/facebook/create-react-app) to start and debug the web app.

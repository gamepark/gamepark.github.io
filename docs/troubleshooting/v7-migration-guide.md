# Migration to framework v7

The Game Park Framework v7 aims to upgrade all the dependencies used to modernize and fasten the game development.

Key points:

- Create React App (obsolete since 2021) is replaced with Vite.js
  - Starting a dev session and hot reloading is incredibly faster now
  - The technical configuration is also simplified
- Lodash is replaced with es-toolkit (to reduce the size of the bundles)
- React is upgraded from v17 to v19
- The libraries are now build as ES modules, no longer CommonJS
- The line `/** @jsxImportSource @emotion/react */` is no longer required on files using emotion css
- Yarn berry (v4) is used instead of Yarn v1

## Step-by-step guide

### Upgrade your tools

- Make sure you have the latest version of NodeJS installed (v22+)
- Install the lastest version of yarn and enable corepack (`corepack enable`)

### Migrate .gitignore and package.json files

- Delete the folder `/app/build`
- Replace .gitignore file with the file from the [Board Game Template](https://github.com/gamepark/board-game-template/blob/main/.gitignore)
- Replace all 3 package.json files in you project with the files from the [Board Game Template](https://github.com/gamepark/board-game-template)
- Replace in those files:
  - `Game Template` => `Name of your Game`
  - `GameTemplate` => `NameOfYourGame`
  - `game-template` => `name-of-your-game`
- run `yarn install`

Here, you should have changes in the `yarn.lock` file, and one new file: `.yarnrc.yml`

### Migrate ts.config files

- Replace the 3 `tsconfig.json` files with those from the [Board Game Template](https://github.com/gamepark/board-game-template)
- Add `/app/tsconfig.app.json` and `/app/tsconfig.node.json` from the template

### Migrate Create React App to Vite config

- Delete `/app/.env`, `/app/config-overrides.js`, `/app/public/index.html`, `/app/src/react-app-env.d.ts`
- Add `/app/index.html` from the [Board Game Template](https://github.com/gamepark/board-game-template/blob/main/app/index.html)
- Rename the title inside `/app/index.html` with the name of you game
- Add `/app/vite.config.ts` and `/app/src/vite-env.d.ts`
- Replace line 15 the alias with the name of you game in `/app/vite.config.ts`
- Add `/app/src/main.tsx` from the [Board Game Template](https://github.com/gamepark/board-game-template/blob/main/app/src/main.html)
  - Replace the default GameProvider with the one from the exiting `/app/src/index.tsx` file
  - Report any extra instruction such as `addStylesheetUrl` from index.tsx to main.tsx.
  - Delete `/app/src/index.tsx`

### Create or update the eslint config

- Copy `eslint.config.js` files from the Board Game Template (in /app and /rules)
- Remove any preexisting `eslint.config.mjs` from the project
- run `yarn lint` and fix any error

### Cleanup the app/public folder

- Delete everything except:
  - `avatar-[].png`, `player-id-[].png`, `rules-[].pdf` files
  - `box-640.png`, `box-640.webp`, `cover-1920.jpg`, `cover-1920.webp`
- Regenerate the favicon from avatar-320.png using https://realfavicongenerator.net/
  - Only take `favicon.ico`, `favicon.svg` and `favicon-96x96.png`
- Copy `robots.txt` from the [Board Game Template](https://github.com/gamepark/board-game-template/blob/main/app/public/robots.txt)

### Replace lodash with es-toolkit

- Research and replace in the project all the dependencies to lodash:
  - `import { xxx } from 'lodash'` => `import { xxx } from 'es-toolkit'` (or `es-toolkit/compat`)
  - `import xxx from 'lodash/xxx` => `import { xxx } from 'es-toolkit'` (or `es-toolkit/compat`)

### Simplify jsxImportSource

- `/** @jsxImportSource @emotion/react */` can be removed everywhere

### Fix PNG images

- PNG images with transparency are no longer automatically detected. You need to add `transparency = true` in the Material Description to prevent the box shadow to appear.
  - You can use `hasTransparency(itemId: ItemId)` if needed

### Upgrade react-i18next usage

- Replace `<Trans defaults=` with `<Trans i18nKey=` everywhere

### Test it!

- Test the tutorial and complete the game to check that everything is fine :)

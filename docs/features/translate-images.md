# Translate images

If you have images with texts, you will have to translate those texts.

## Translation inside the help dialog

All the texts visible on your items must be repeated inside the help dialog. This way, even if the image is not translated, the translated text will be available in the help dialog.

## Translation of the image

Although it is possible to ask for images without texts and inject the text on top of the images using HTML and CSS, this approach is very time consuming and the final quality is never satisfying: the text can overflow in other languages, the font used is rarely available, etc.

For this reason, the preferred solution is to add the images in multiple languages.

1. Add the translated images in the images folder (you can organize them by language: `images/cards/en` and `images/cards/fr` for instance).
2. Create a new MaterialDescription class that extends the original one, like in [Chateau Combo](https://github.com/gamepark/chateau-combo/blob/main/app/src/material/FrenchChateauComboCardDescription.tsx).
3. Inject a new materialI18 property in the [GameProvider](https://github.com/gamepark/chateau-combo/blob/main/app/src/index.tsx).

:bulb: The images will automatically match the language inside the "locale" param in the url (try for example https://localhost:3000/?locale=fr). Only the right images are preloaded and used so it does not increase the loading time of the game.

The translated images are usually provided by the published. The main description acts as the fallback if the user uses a language without translated images.

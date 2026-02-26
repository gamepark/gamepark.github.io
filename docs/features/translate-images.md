# Translate images

If you have images with texts, you will have to translate those texts.

## Translation inside the help dialog

All the texts visible on your items must be repeated inside the help dialog. This way, even if the image is not translated, the translated text will be available in the help dialog.

## Translation of the image

Although it is possible to ask for images without texts and inject the text on top of the images using HTML and CSS, this approach is very time consuming and the final quality is never satisfying: the text can overflow in other languages, the font used is rarely available, etc.

For this reason, the preferred solution is to add the images in multiple languages.

### 1. Add the translated images

Organize your images by language in the images folder:

```
app/src/images/cards/
├── en/
│   ├── Card_1_EN_FirstCard.jpg
│   └── Card_2_EN_SecondCard.jpg
└── fr/
    ├── Card_1_FR_FirstCard.jpg
    └── Card_2_FR_SecondCard.jpg
```

### 2. Create a localized MaterialDescription

Create a new MaterialDescription class that extends the original one and overrides the `images` property:

```typescript
// FrenchCardDescription.ts
import { Card } from '@gamepark/my-game/material/Card'
import { MyCardDescription } from './MyCardDescription'
import FirstCard_FR from '../images/cards/fr/Card_1_FR_FirstCard.jpg'
import SecondCard_FR from '../images/cards/fr/Card_2_FR_SecondCard.jpg'

class FrenchCardDescription extends MyCardDescription {
  images = {
    [Card.FirstCard]: FirstCard_FR,
    [Card.SecondCard]: SecondCard_FR
  }
}

export const frenchCardDescription = new FrenchCardDescription()
```

The class inherits everything from the base description (dimensions, back images, etc.) and only overrides the front images.

See [Skyrift](https://github.com/gamepark/skyrift/blob/main/app/src/material/FrenchCardDescription.ts) for a complete example.

### 3. Export `materialI18n` and inject it in the GameProvider

In `Material.ts`, export a `materialI18n` object that maps locale codes to partial material descriptions:

```typescript
// Material.ts
import { frenchCardDescription } from './FrenchCardDescription'

export const materialI18n: Record<string, Partial<Record<MaterialType, MaterialDescription>>> = {
  fr: {
    [MaterialType.Card]: frenchCardDescription
  }
}
```

Then pass it to the `GameProvider` in `main.tsx`:

```
import { Material, materialI18n } from './material/Material'

<GameProvider
  material={Material}
  materialI18n={materialI18n}
  // ...other props
>
```

:bulb: The images will automatically match the language inside the "locale" param in the url (try for example https://localhost:3000/?locale=fr). Only the right images are preloaded and used so it does not increase the loading time of the game.

The translated images are usually provided by the publisher. The main description acts as the fallback if the user uses a language without translated images.

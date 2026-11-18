# Prepare the images

## Size of images

**1 centimeter = 100 pixels**

We use that convention as it is a good tradeoff between bandwidth usage and image quality.

## File types

**Use WebP for every image.** The build converts each imported image to WebP anyway (see below), so a JPG
or a PNG source is compressed twice, once by you and once by the build, for no gain. Save your images as
**lossless WebP**: the build does the lossy compression, and it does it on a pristine source.

For images with regular shapes (rectangular shapes like cards, boards and most tokens), an opaque image is
enough. Rounded borders and shadows are done using CSS.

For images that are irregular, use transparency, and include the shadow in the image:
- Size the image like usual
- Add 10 pixels on every side
- Add a 10 pixels all-around shadow

:warning: To get the right images ratio, you need to consider that your item is now 0.2 centimeters higher and larger.

:bulb: `vite-imagetools` converts **every** image imported from `app/src` to WebP at build time, whatever
its source format, to save bandwidth.

:warning: The game therefore ships WebP only, with no fallback to the original format: on a browser
without WebP support, the image is not displayed at all. WebP is supported by Chrome and Edge since 2018,
Firefox since 2019 and Safari since 2020 (Safari 14, iOS 14).

## Folder for images

By convention, we use app/src/images to store all the images. We create subfolders per categories or types of items when needed.

For example, in [Architects of Amytis](https://github.com/gamepark/architects-of-amytis/tree/main/app/src/images) we created 4 folder: boards, cards, resources and tiles.

## Images names

It is recommended to name the images in CamelCase.

For items unique in their type, the name can be the same of the MaterialType, like for [Architects of Amytis boards](https://github.com/gamepark/architects-of-amytis/tree/main/app/src/images/boards).

For items with many images in their type, the name can be the same as the [future item's id](step-by-step-example/items-id.md). This way, the future mapping will be easier: have a look at [the building tiles images mapping in Architects of Amytis](https://github.com/gamepark/architects-of-amytis/blob/main/app/src/material/BuildingTileDescription.tsx) for example.

## Images with texts

Texts of images cannot be translated. We could use images with the text removed, and add the translated texts later using code, but it requires a lot of work and the result is never perfect.

Therefore, we use images with the texts, and if the images are available in multiple language we use the [Material internationalization feature](features/translate-images.md).
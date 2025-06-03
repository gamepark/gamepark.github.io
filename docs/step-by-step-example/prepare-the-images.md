# Prepare the images

## Size of images

**1 centimeter = 100 pixels**

We use that convention as it is a good tradeoff between bandwidth usage and image quality.

## File types

For image with regular shapes (rectangular shapes like cards, boards and most tokens), use JPG format. Rounded borders and shadows are done using CSS.

For images that are irregular, use PNG with transparency, and shadow included:
- Size the image like usual
- Add 10 pixels on every side
- Add a 10 pixels all-around shadow

:warn: To get the right images ratio, you need to consider that your item is now 0.2 centimeters higher and larger.

:bulb: The framework will automatically generate webp versions of every image to save bandwidth.

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
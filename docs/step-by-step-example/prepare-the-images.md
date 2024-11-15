# Prepare the images

## Size of images

**1 centimeter = 100 pixels**

We use that convention as it is a good tradeoff between bandwidth usage and image quality.

## File types

For image with regular shapes (rectangular shapes like cards, boards and most tokens), use JPG format. Rounded borders and shadows are done using CSS.

For images that are irregular, use PNG with transparency, and shadow included:
- Size the image like usual
- Add 10 pixels on every side
- Add a 10px bottom-left shadow

:bulb: The framework will automatically generate webp versions of every image to save bandwidth.

## Folder for images

By convention, we use app/src/images to store all the images. We create subfolders per categories or types of items when needed.
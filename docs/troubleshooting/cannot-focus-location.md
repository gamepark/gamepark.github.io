# Cannot focus location

Inside the tutorial, you can focus items, static items and locations.

The focus system will count the number of HTML elements expected to be focused, and the focus only works if every element can be found.

If you are focusing a location that usually does not exist, it will automatically be created on the fly so that it exists. However, locations that does not belong to a parent item do not have a size by default: when it does not have a location description, the location is automatically created using the size of the dragged item when it is a valid drop location.

Therefore, if you want to focus a location that is only created "on the fly" as a drop location when dragging an item, you have to specify a location description for that locator and explicitly define its size:

```typescript jsx
locationDescription = new DropAreaDescription(someItemDescription)
```

:bulb: if the location has a parent item, its default size is the size of the parent item.

###Use the editor widget to update a feature layer

In this lab you will create an app that allows users to add, edit, or delete features.  It uses the editor widget, which is new as of version 4.11.  The application links to a web map that contains an editable layer from ArcGIS Online.

1.	Click [create_starter_map/index.html](../create_starter_map/index.html) and copy the contents to a new [jsbin.com](http://jsbin.com).

2.	Add the following css to the style.  This code adds a scrollbar to the list of editable attributes if the size exceeds 350px:

```css
.esri-editor .esri-item-list__scroller {
  max-height: 350px;
}
```

3.	Update the require statement and function definition:

```javascript
require([
  "esri/WebMap",
  "esri/views/MapView",
  "esri/widgets/Editor"
], function(WebMap, MapView, Editor) {
```

4.	Change the map to a WebMap and edit the view.

```javascript
var webmap = new WebMap({
  portalItem: {
    id: "325015b250b04602be82499a8ac62a59"
  }
});

var view = new MapView({
  container: "viewDiv",
  map: webmap
});
```

5.	When the view is ready, disable popups, create the editor widget, and add it to the top right corner.

```javascript
view.when(function() {
  view.popup.autoOpenEnabled = false; 
  var editor = new Editor({
    view: view
  });
  view.ui.add(editor, "top-right");
});
```

6.	Finally, add the editor to the html.

```html
<div id="editorDiv"></div>
```

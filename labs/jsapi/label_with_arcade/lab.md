###Use Arcade to create custom labels

In this lab you will use Arcade to create custom labels based on each feature’s attributes.  Through arcade you can access individual features by using $feature, and use it to edit lables or rendering.  Arcade scripts can be used across the Esri platform to include ArcGIS Pro, the JavaScript API, and mobile applications.

1.	Click [create_starter_map/index.html](../create_starter_map/index.html) and copy the contents to a new [jsbin.com](http://jsbin.com).

2.	Add the following code directly below </script>.  This is the Arcade expression that will be used to produce the label.

```javascript
<script type="text/plain" id="amenities">
  var i = 0;
  var features = [];
      
  if ($feature.TastingRoo != " ") {
    features[i++] = "-Tasting Room Onsite";
  }
  if ($feature.GroupTour != " ") {
    features[i++] = "-Tours Available";
  }
  if ($feature.PicnicArea != " ") {
    features[i++] = "-Picnic Area Onsite";
  }
  if ($feature.FoodAvaila != " ") {
    features[i++] = "-Food Available Onsite";
  }
      
  return Concatenate(features, TextFormatting.NewLine)
</script>
```

3.	Update the require statement and function definition:

```javascript
require([
  "esri/Map",
  "esri/views/MapView",
  "esri/layers/FeatureLayer",
  "esri/geometry/Extent"
], function(Map, MapView, FeatureLayer, Extent) {
```

4.	Change your map and view.  Add a variable that will be used to determine at what scale labels should draw.

```javascript
var minScale = 100000;
var map = new Map({
  basemap: "gray-vector"     
})
 
var extent = new Extent ({
  xmin: -83.7,
  ymin: 36.4,
  xmax: -75.7,
  ymax: 39.6
})
 
var view = new MapView({
  container: "viewDiv",
  map: map,
  extent: extent
});
```

5.	Create two labeling classes.  One is for the feature name and will be displayed at the top-right corner.  The second, which uses the arcade script, is for the list of amenities and will be displayed to the bottom-right of the feature.  Both classes call a function named createTextSymbol which determines how the label is displayed.

```javascript
var nameClass = {
  labelPlacement: "above-right",
  labelExpressionInfo: {
    expression: "$feature.NAME"
  },
  minScale: minScale
};
nameClass.symbol = createTextSymbol("black");
        
var amenitiesArcade = document.getElementById("amenities").text;
var amenitiesClass = {
  labelExpressionInfo: {
    expression: amenitiesArcade
  },
  labelPlacement: "below-right",
    minScale: minScale
  };
amenitiesClass.symbol = createTextSymbol("#3ba53f");
```

6.	Create the feature layer and add it to the map.

```javascript
var serviceUrl = "https://services5.arcgis.com/cMBqYE4wmbXNMvex/ArcGIS/rest/services/Virginia_Wineries/FeatureServer/1";
var layer = new FeatureLayer({
  url: serviceUrl,
  renderer: {
    type: "simple",
    symbol: {
      type: "simple-marker",
      color: [128, 0, 128, 0.6],
      size: 10,
      outline: {
        color: [0, 0, 0, 0.4],
        width: 0.5
      }
    }
  },
  labelingInfo: [
    nameClass,
    amenitiesClass
  ]
});

view.map.add(layer);
```

7.	The last thing we need to do is to add the createTextSymbol function:

```javascript
function createTextSymbol(color) {
  return {
    type: "text",
    font: {
      size: 12,
      weight: "bold"
    },
    color: "white",
    haloColor: color,
    haloSize: 1
  };
}
 ```
 
 Your app should display a map of all the wineries in Virginia.  They shouod be labeled with the winery name and list the available amenities.
 
 It should look something like this:
* [Code](index.html)
* [Live App](http://jofraley.github.io/Hacking_JavaScript/labs/jsapi/label_with_arcade/index.html)

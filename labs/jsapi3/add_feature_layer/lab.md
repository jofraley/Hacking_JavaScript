###Add a feature layer to a map

In this lab you will add a feature layer to an ArcGIS API for JavaScript application. 

1. Click [create_starter_map/index.html](../create_starter_map/index.html) and copy the contents to a new [jsbin.com](http://jsbin.com).

2. In `JSBin` > `HTML`, update the `require` statement and function definition:

  ```javascript
  require(["esri/map",
           // ADD module
           "esri/layers/FeatureLayer", 
           "dojo/domReady!"],
    // ADD FeatureLayer reference
    function(Map, FeatureLayer) {
      ...
  ```

3. Now add a new `FeatureLayer` to the map:

  ```javascript
  function(Map, FeatureLayer) {
    map = new Map("mapDiv", {
      center: [-77.029, 38.89],
      zoom: 12,
      basemap: "dark-gray"
    });

    // ADD a feature layer
    var featureLayer = new FeatureLayer("https://services.arcgis.com/lA2FZKuu26Fips7U/arcgis/rest/services/BlockGroupsDC/FeatureServer/0");

    map.addLayer(featureLayer);
  ```

4. Confirm that the JSBin `Output` panel shows a map with Washington DC block groups.

Your app should look something like this:
* [Code](index.html)
* [Live App](https://jofraley.github.io/Hacking_JavaScript/labs/jsapi3/add_feature_layer/index.html)


###Bonus
* Add the [Metro Stops feature layer](https://services.arcgis.com/lA2FZKuu26Fips7U/arcgis/rest/services/MetroStops/FeatureServer/0) to the map,
 and then add the [Metro Lines feature layer](https://services.arcgis.com/lA2FZKuu26Fips7U/arcgis/rest/services/MetroLines/FeatureServer/0).
* Ensure the layers are ordered with polygons on the bottom, lines and then points on top.
* Make another application and add this image service https://sampleserver6.arcgisonline.com/arcgis/rest/services/Toronto/ImageServer

  [Here is the result](https://jofraley.github.io/Hacking_JavaScript/labs/jsapi3/add_image_layer/index.html)

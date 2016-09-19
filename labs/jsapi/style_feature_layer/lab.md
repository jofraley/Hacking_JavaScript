###Style Feature Layers

In this lab you will apply custom styling to a feature layer.

1. Click [create_starter_map/index.html](../create_starter_map/index.html) and copy the contents to a new [jsbin.com](http://jsbin.com).

2. In `JSBin` > `HTML`, update the `require` statement and `function` definition.

  ```javascript
  require([
    "esri/Map",
    "esri/views/MapView",
    /*** ADD ***/
    "esri/layers/FeatureLayer",
    "esri/symbols/SimpleMarkerSymbol",
    "esri/renderers/UniqueValueRenderer",
    "dojo/domReady!"
  ], function(Map, MapView, FeatureLayer, SimpleMarkerSymboll, UniqueValueRenderer) {
  ```

3. Now set up a `UniqueValueRenderer` based off the `Transfer` field.

  ```javascript
    ...

    var view = new MapView({
      container: "viewDiv",
      map: map,
      center: [-77.029, 38.89],
      zoom: 10
    });

    /*** ADD ***/
    var renderer = new UniqueValueRenderer({
      field: "TYPE",
      defaultSymbol: new SimpleMarkerSymbol()
    });
  ```

4. Next we tell the renderer how to show each `Transfer` value (the values are `Yes` or `No`). We want to highlight `Yes`, so we make this a diamond and a little bigger size.

  ```javascript
    var renderer = new UniqueValueRenderer({
        field: "Transfer",
        defaultSymbol: new SimpleMarkerSymbol()
      });
      renderer.addUniqueValueInfo("Yes",
        new SimpleMarkerSymbol({
          color: [0, 0, 0, 0.8],
          size: 13,
          style: "diamond",
          outline: {
            color: [255, 255, 255],
            width: "1px"
          }
        })
      );
      renderer.addUniqueValueInfo("No",
        new SimpleMarkerSymbol({
          color: [255, 255, 255, 0.8],
          size: 8
        })
      );
  ```

5. Lastly, we create the `FeatureLayer`, attach the `UniqueValueRenderer`, and add it to the map.

  ```javascript
    /*** ADD ***/
    var featureLayer = new FeatureLayer({
      url: "http://services.arcgis.com/lA2FZKuu26Fips7U/ArcGIS/rest/services/MetroStops/FeatureServer/0",
      renderer: renderer 
    });

    map.add(featureLayer);
  ```

Your app should look something like this:
 * [Code](index.html)
 * [Live App](http://jofraley.github.io/Hacking_JavaScript/labs/jsapi/style_feature_layer/index.html)

###Bonus
 * Add a [Metro Lines feature layer](http://services.arcgis.com/lA2FZKuu26Fips7U/ArcGIS/rest/services/MetroLines/FeatureServer/0) to the map and then apply custom styles to it.

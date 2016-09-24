###Style Feature Layers

In this lab you will apply custom styling to a feature layer.

1. Click [create_starter_map/index.html](../create_starter_map/index.html) and copy the contents to a new [jsbin.com](http://jsbin.com).

2. In `JSBin` > `HTML`, update the `require` statement and function definition.

  ```javascript
  require(["esri/map",
           // ADD modules 
           "esri/Color",
            "esri/symbols/SimpleFillSymbol",
            "esri/layers/FeatureLayer", "esri/renderers/ClassBreaksRenderer",
            "dojo/domReady!"],
    // ADD FeatureLayer,SimpleLineSymbol,ClassBreaksRenderer,Color references
    function(Map,Color,SimpleFillSymbol,FeatureLayer, ClassBreaksRenderer) {
      ...
  ```

3. Now set up a `ClassBreaksRenderer` based off the `P0010001` field.

  ```javascript
  function(Map,Color,SimpleFillSymbol,FeatureLayer, ClassBreaksRenderer) {
    map = new Map("mapDiv", {
      center: [-77.029, 38.89],
      zoom: 12,
      basemap: "dark-gray"
    });

    // ADD a Class Break Renderer with no default symbol
    var renderer = new ClassBreaksRenderer(null, "P0010001");
  ```

4. Next we tell the renderer how to show each class break for `P0010001`.

  ```javascript
 renderer.addBreak(0, 500, new SimpleFillSymbol().setColor(new Color([204, 255, 204, 0.6])));
 renderer.addBreak(500, 1500, new SimpleFillSymbol().setColor(new Color([164, 245, 157, 0.6])));
 renderer.addBreak(1500, 2500, new SimpleFillSymbol().setColor(new Color([123, 232, 111, 0.6])));
 renderer.addBreak(2500, 3500, new SimpleFillSymbol().setColor(new Color([77, 217, 67, 0.6])));
 renderer.addBreak(3500, Infinity, new SimpleFillSymbol().setColor(new Color([14, 204, 14, 0.6])));
  ```

5. Lastly, we create the `FeatureLayer`, attach the `ClassBreaksRenderer`, and add it to the map. Because the renderer relies on the `P0010001` field, we tell the `FeatureLayer` to retrieve it by specifying the `outFields` parameter.

  ```javascript
  var featureLayer = new FeatureLayer("https://services.arcgis.com/lA2FZKuu26Fips7U/arcgis/rest/services/BlockGroupsDC/FeatureServer/0", {
    mode: FeatureLayer.MODE_ONDEMAND,
    outFields: ["P0010001"]
  });

  featureLayer.setRenderer(renderer);

  map.addLayer(featureLayer);
  ```
 
 Your app should look something like this:
 * [Code](index.html)
 * [Live App](http://jofraley.github.io/Hacking_JavaScript/labs/jsapi3/style_feature_layer/index.html)

###Options
 * Add a renderer to the Metro Stops

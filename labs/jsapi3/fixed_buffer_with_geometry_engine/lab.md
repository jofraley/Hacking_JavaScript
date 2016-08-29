###Client-side Buffer by Fixed Amount

In this lab you will use the GeometryEngine to buffer around Rail Stops in the browser by a fixed amount.

1. Click [create_starter_map/index.html](../create_starter_map/index.html) and copy the contents to a new [jsbin.com](http://jsbin.com).

2. In `JSBin` > `HTML`, update the `require` statement and function definition:

    ```javascript
    require(["esri/map",
             // ADD modules
             "esri/layers/GraphicsLayer",
             "esri/layers/FeatureLayer",
             "esri/geometry/geometryEngineAsync",
             "esri/graphic",
             "esri/graphicsUtils",
             "esri/symbols/SimpleFillSymbol",
             "esri/Color",
             "dojo/domReady!"],
      // ADD aliases
      function(Map,
               GraphicsLayer,FeatureLayer,
               geometryEngineAsync,
               Graphic,graphicsUtils,
               SimpleFillSymbol,Color) {
        ...
    ```

3. Add a `FeatureLayer` for the Metro Stops and a `GraphicsLayer` to display the calculated buffers. Optionally modify the map to initialize at zoom level `12`:

    ```javascript
    function(Map,
             GraphicsLayer,FeatureLayer,
             geometryEngineAsync,
             Graphic,graphicsUtils,
             SimpleFillSymbol,Color) {
      map = new Map("mapDiv", {
        center: [-77.029, 38.89],
        zoom: 12,
        basemap: "dark-gray"
      });

      // ADD Create layers and add them to the map
      var bufferLayer = new GraphicsLayer(),
          stopsLayer = new FeatureLayer("http://services.arcgis.com/lA2FZKuu26Fips7U/arcgis/rest/services/MetroStops/FeatureServer/0", {
            mode: FeatureLayer.MODE_SNAPSHOT
          });

      map.addLayers([bufferLayer, stopsLayer]);
    ```

4. Create a buffer symbol and a function to calculate and display a buffer:

    ```javascript
    // ADD Create a symbol to display the buffers
    var bufferSymbol = new SimpleFillSymbol(SimpleFillSymbol.STYLE_SOLID, undefined,
                       new Color([0,100,255,0.4]));

    // ADD A function to buffer the Metro Stops
    map.on('click', function bufferStopsLayer() {
      var stopGeoms = graphicsUtils.getGeometries(stopsLayer.graphics);
      geometryEngineAsync.union(stopGeoms).then(function (stops) {
        geometryEngineAsync.geodesicBuffer(stops, 0.5, 'miles').then(function (totalBuffer) {
          bufferLayer.clear();
          bufferLayer.add(new Graphic(totalBuffer, bufferSymbol));
        });
      });
    });
    ```

5. In JSBin, run the app > Click on the map to buffer the rail stops by 0.5 miles.

Your app should look something like this:
* [Code](index.html)
* [Live App](http://jofraley.github.io/Hacking_JavaScript/labs/jsapi3/fixed_buffer_with_geometry_engine/index.html)

###Bonus
* See the [Interactive Buffer Lab](../buffer_with_geometry_engine/lab.md) for more bonus items.

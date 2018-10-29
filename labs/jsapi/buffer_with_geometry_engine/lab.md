###Client-side Buffer

In this lab you will use the GeometryEngine to buffer around Rail Stops in the browser by a fixed amount.

1. Click [create_starter_map/index.html](../create_starter_map/index.html) and copy the contents to a new [jsbin.com](http://jsbin.com).

2. Update the `require` statement and `function` definition:

 ```javascript
  require([
      "esri/Map",
      "esri/views/MapView",
      "esri/layers/GraphicsLayer",
      "esri/Graphic",
      "esri/symbols/SimpleFillSymbol",
      "esri/geometry/geometryEngine",
      "dojo/domReady!"
    ], function(Map, MapView,
                GraphicsLayer, Graphic, SimpleFillSymbol, geometryEngine) {
 ```

3. Add a graphics layer to the map.

  ```javascript
   var bufferLayer = new GraphicsLayer();
      
	  map.add(bufferLayer);
  ```

4. We need a symbol for the buffer and the point where you click on the map.

  ```javascript
    var bufferSymbol = new SimpleFillSymbol({
        color: [0,100,255,0.4],
        style: "solid",
        outline: {
          color: [110,110,110],
          width: 1
        }
      });
	  
	  var pointSym = {
        type: "simple-marker", // autocasts as new SimpleMarkerSymbol()
        color: [255, 0, 0],
        outline: {
          color: [255, 255, 255],
          width: 1
        },
        size: 7
      };
  ```

5. We need to capture the event of clicking on the view and call a funtion to generate the cuffer.

  ```javascript
    view.on("click", function(event) {
        bufferPoint(event.mapPoint);
      });
	  
	  function bufferPoint(point) {
        bufferLayer.add(new Graphic({
          geometry: point,
          symbol: pointSym
        }));

        var buffer = geometryEngine.geodesicBuffer(point, 1, "miles");
        bufferLayer.add(new Graphic({
          geometry: buffer,
          symbol: bufferSymbol
        }));
      }
  ```

6. In JSBin, run the app > When the stops have loaded, a 0.5 mile buffer will be calculated around them and added to the map.

Your app should look something like this:
* [Code](index.html)
* [Live App](http://jofraley.github.io/Hacking_JavaScript/labs/jsapi/buffer_with_geometry_engine/index.html)

###Bonus
* Modify buffer distance and units.
* Clear the graphics from previous click on the map.
  * [Code](index_clear.htmll)
  * [Live App](http://jofraley.github.io/Hacking_JavaScript/labs/jsapi/buffer_with_geometry_engine/index_clear.html)

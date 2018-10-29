###Calling a Geoprocessing Service

In this lab you will call a geoprocessing service to calculate a viewshed.

1. Click [create_starter_map/index.html](../create_starter_map/index.html) and copy the contents to a new [jsbin.com](http://jsbin.com).

2. Update the `require` statement and `function` definition:

  ```javascript
     require([
	  "esri/Map",
          "esri/views/MapView",
          "esri/layers/GraphicsLayer",
	  "esri/Graphic",
	  "esri/geometry/Point",
	  "esri/tasks/Geoprocessor",
	  "esri/tasks/support/LinearUnit",
	  "esri/tasks/support/FeatureSet",
	  "dojo/domReady!"
    ], function(Map, MapView, GraphicsLayer, Graphic, Point,
        Geoprocessor, LinearUnit, FeatureSet, domReady) { 
  ```

3. Next update the map to use the hybrid basemap and the view to center on a new location. Also add a graphic layer to the map

  ```javascript
	  var map = new Map({
          basemap: "hybrid",
      });

	  var view = new MapView({
          container: "viewDiv",
          map: map,
          center: [7.59564, 46.06595],
		  zoom: 12
        });
	var graphicsLayer = new GraphicsLayer();
        map.add(graphicsLayer);
  ```

4. We are going to define a couple symbols for use for where the user clicked on the map and the results of the viewshed.
  ```javascript
    var markerSymbol = {
          type: "simple-marker", // autocasts as new SimpleMarkerSymbol()
          color: [255, 0, 0],
          outline: { // autocasts as new SimpleLineSymbol()
            color: [255, 255, 255],
            width: 2
          }
        };

        var fillSymbol = {
          type: "simple-fill", // autocasts as new SimpleFillSymbol()
          color: [0, 255, 0, 0.75],
          outline: { // autocasts as new SimpleLineSymbol()
            color: [255, 255, 255],
            width: 1
          }
        };
  ```

5. Now we can define the geoprocessor and set the url and out spatial reference.  And then listen for the view on click event to trigger calling the computeViewshed function.

  ```javascript
    var gp = new    Geoprocessor("https://sampleserver6.arcgisonline.com/arcgis/rest/services/Elevation/ESRI_Elevation_World/GPServer/Viewshed");
        gp.outSpatialReference = { // autocasts as new SpatialReference()
          wkid: 102100
        };
        view.on("click", computeViewshed);
  ```
6. What does the computeViewshed function look like and do?  First the function takes the event where the user clicks on the map.  We need to remove all the existing graphics before we exectue again.  Then we create a new point and sets its symbol and add it to the graphics layer.  The geoprocessing service takes a featureset as an input parameter so we need to put our point into that featureset.  Another parameter is linearunit for the distance and units to calculate the viewshed.  We set the parameters that we need to pass and then we call execute on the geoprocessor.  Once it is finished it will call another function drawResultData.
```javascript
        function computeViewshed(event) {
          graphicsLayer.removeAll();

          var point = new Point({
            longitude: event.mapPoint.longitude,
            latitude: event.mapPoint.latitude
          });

          var inputGraphic = new Graphic({
            geometry: point,
            symbol: markerSymbol
          });

          graphicsLayer.add(inputGraphic);

          var inputGraphicContainer = [];
          inputGraphicContainer.push(inputGraphic);
          var featureSet = new FeatureSet();
          featureSet.features = inputGraphicContainer;

          var vsDistance = new LinearUnit();
          vsDistance.distance = 5;
          vsDistance.units = "miles";

          var params = {
            "Input_Observation_Point": featureSet,
            "Viewshed_Distance": vsDistance
          };

          gp.execute(params).then(drawResultData);
        }
```
7. Finally we need to handle the results of the viewshed.  We get the results and need to define their symbol and then add the graphics to the map.  Last we want to zoom to the graphics.
```javascript
	function drawResultData(result) {
          var resultFeatures = result.results[0].value.features;

          // Assign each resulting graphic a symbol
          var viewshedGraphics = resultFeatures.map(function(feature) {
            feature.symbol = fillSymbol;
            return feature;
          });

          // Add the resulting graphics to the graphics layer
          graphicsLayer.addMany(viewshedGraphics);

          view.goTo({
            target: viewshedGraphics
          });
        }
```
8. In JSBin, run the app > Select a category to query the feature layer. Click on any point to display the attribute data in a popup.

Your app should look something like this:
* [Code](index.html)
* [Live App](http://jofraley.github.io/Hacking_JavaScript/labs/jsapi/gp_service/index.html)

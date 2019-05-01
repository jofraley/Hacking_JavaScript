###Extrude a polygon in 3D

This lab introduces visual variables as a powerful way of extruding polygons in a 3D mapping application.

1. Copy and paste the code below into a new [jsbin.com](http://jsbin.com).

  ``` html 
  <!DOCTYPE html>
  <html>
  <head>
    <meta charset="utf-8">
    <meta name="viewport" content="initial-scale=1,maximum-scale=1,user-scalable=no">
    <title>Extrude polygon by visual variables</title>

    <link rel="stylesheet" href="https://js.arcgis.com/4.11/esri/css/main.css">
    <script src="https://js.arcgis.com/4.11/"></script>
    
    <style>
      html, body, #viewDiv {
        padding: 0;
        margin: 0;
        height: 100%;
        width: 100%;
      }
    </style>

    <script>
      require(["esri/Map",
               "esri/views/SceneView",
               "esri/layers/FeatureLayer",
               "esri/Color",
               "esri/symbols/PolygonSymbol3D",
               "esri/symbols/ExtrudeSymbol3DLayer",
               "esri/renderers/SimpleRenderer",
               "dojo/domReady!"], 
        function(Map, SceneView, FeatureLayer, 
                 Color, PolygonSymbol3D, ExtrudeSymbol3DLayer, SimpleRenderer) {

          //Create map
          var map = new Map({
            basemap: "terrain"
          });

          //Create SceneView for 3d visualization
          var view = new SceneView({
            container: "viewDiv",
            map: map,
            camera: {
              position: [-74.0060, 40.328, 30000],
              tilt: 50,
              heading: 0
            }
          });

        }
      );
    </script>
  </head>
  <body>
    <div id="viewDiv"></div>
  </body>
  </html>
  ```
   
2. The JSBin `Output` panel should show a 3D view of earth that you can rotate around.

3. Below the `SceneView` add a feature layer. When you test run your app the layer will look like blue polygons. 
Don't worry, we're going to fix this in just a minute.


  ``` javascript
      //Create featureLayer and add to the map
      var blockgroups = new FeatureLayer({
        url: "https://services.arcgis.com/bGgB6gXiQ835YdNp/arcgis/rest/services/NYC_All_Neighborhoods_Alc/FeatureServer/0"
      });
      map.add(blockgroups);
  ```
   
4. Now lets add a `SimpleRenderer` to the feature layer. Since this is 3D we are going to use a `PolygonSymbol3D` and 
then we'll apply `visualVariables` to define how to render the values by color, size and opacity.

   
  ``` js
      //Create the Renderer for the featureLayer,
      var extrudePolygonRenderer = new SimpleRenderer({
        symbol: new PolygonSymbol3D({
          symbolLayers: [new ExtrudeSymbol3DLayer()]
        }),
        // These define how to render by size, color and/or opacity
        // Each visualVariable is associated with a field
        visualVariables: [{
          // Total Wine Consumption -- symbolize by polygon height
            type: "size",
            field: "food_x2005_x",
            stops: [
              {
                value: 0,
                size: 10
              },
              {
                value: 20000000,
                size: 2000
              }]
          }, {
            // Total Beer Consumption -- Symbolize by color
            type: "color", 
            field: "food_x2003_x",
            stops: [
              {
                value: 0,
                color: "#FFFCD4",
              },
              {
                value: 20000000,
                color: "#0D2644",
              }]
          }
        ]
        });
        blockgroups.renderer = extrudePolygonRenderer;
  ```
   
Your app should look something like this:

 * [Code](https://github.com/jofraley/Hacking_JavaScript/tree/master/labs/jsapi/extrude_polygon_3d)
 * [Live App](http://jofraley.github.io/Hacking_JavaScript/labs/jsapi/extrude_polygon_3d/index.html)
 
###Bonus

* Experiment with Visual Variables. You can read mode about that [here](https://developers.arcgis.com/javascript/latest/api-reference/esri-renderers-SimpleRenderer.html#visualVariables).
* Experiment with opacity values. You can read more about that [here](https://developers.arcgis.com/javascript/latest/api-reference/esri-renderers-SimpleRenderer.html#opacity).

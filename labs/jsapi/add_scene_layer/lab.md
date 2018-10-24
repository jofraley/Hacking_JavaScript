###Add a Scene Layer to a 3D map

This lab covers the basics of adding a scene layer to your application.
If you are new to ArcGIS and need a full set of instructions on building a basic 3D mapping application
visit the [Get started with SceneView](https://developers.arcgis.com/javascript/latest/sample-code/get-started-sceneview/index.html) tutorial.
To learn more about what options you have with SceneLayers look at the [SceneLayer API Reference](https://developers.arcgis.com/javascript/latest/api-reference/esri-layers-SceneLayer.html)

1. Copy and paste the code below into a new [jsbin.com](http://jsbin.com).

  ```html 
  <!DOCTYPE html>
  <html>
  <head>
    <meta charset="utf-8">
    <meta name="viewport" content="initial-scale=1,maximum-scale=1,user-scalable=no">
    <title>3D Map with SceneLayer</title>

    <link rel="stylesheet" href="https://js.arcgis.com/4.9/esri/css/main.css">
    <script src="https://js.arcgis.com/4.9/"></script>

    <style>
      html, body, #viewDiv {
        padding: 0;
        margin: 0;
        height: 100%;
      }
    </style>

    <script>
      require([
        "esri/Map",
        "esri/views/SceneView",
        "esri/layers/SceneLayer",
        "dojo/domReady!"
      ], function(Map, SceneView, SceneLayer) {

	    var buildingsSL = new SceneLayer({
			url: "https://tiles.arcgis.com/tiles/hRUr1F8lE8Jq2uJo/arcgis/rest/services/BuildingsDC/SceneServer/layers/0"
		});
		
        var map = new Map({
          basemap: "dark-gray-vector",
		      layers: [buildingsSL],
          ground: "world-elevation"
        });
        
        var view = new SceneView({
          container: "viewDiv",
          map: map,
          center: [-77.029, 38.89],
          scale: 50000000
        });

      });
    </script>
  </head>
  <body>
    <div id="viewDiv"></div>
  </body>
  </html>
  ```

2. The JSBin `Output` panel should show a 3D view of earth that you can rotate around and zoom in on DC.


3. Since we are focused on DC, modify the SceneView to be zoomed in more and tilt the scene since we are looking at 3D data. Take a look at the SceneView and the [Camera](https://developers.arcgis.com/javascript/latest/api-reference/esri-views-SceneView.html#camera

```html 
 var view = new SceneView({
   container: "viewDiv",
   map: map,
   camera:{
     position: [-77.04, 38.86, 790],
     tilt: 71.47,
     heading: 30.25
   }
 });
```
Your app should look something like this:

 * [Code](index.html)
 * [Live App](https://jofraley.github.io/Hacking_JavaScript/labs/jsapi/add_scene_layer/index.html)

###Bonus

* Add the same layers you did in the 2D map, [MetroLines](https://services.arcgis.com/hRUr1F8lE8Jq2uJo/arcgis/rest/services/Metro_Lines_Regional/FeatureServer/0), [BlockGroups](https://services.arcgis.com/hRUr1F8lE8Jq2uJo/arcgis/rest/services/Census_Block_Groups__2010/FeatureServer/0), and [MetroStations](https://services.arcgis.com/hRUr1F8lE8Jq2uJo/arcgis/rest/services/Metro_Stations_Regional/FeatureServer/0)
* 

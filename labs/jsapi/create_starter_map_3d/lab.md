### Exercise 3 Build a starter 3D map

This lab covers the basics for creating a basic starter 3D mapping application.
The starter map simply loads a default base map and centers it.

* Explore the [SceneView Class and its corresponding properties](https://developers.arcgis.com/javascript/latest/api-reference/esri-views-SceneView.html)
* Explore the [Map Class and its corresponding properties](https://developers.arcgis.com/javascript/latest/api-reference/esri-Map.html)



1. Copy and paste the code below into a new [jsbin.com](http://jsbin.com).

  ```html 
  <!DOCTYPE html>
  <html>
  <head>
    <meta charset="utf-8">
    <meta name="viewport" content="initial-scale=1,maximum-scale=1,user-scalable=no">
    <title>Exercise 3 -- Creating a 3D Scene</title>

    <!-- Add references to latest ArcGIS API for Javascript Version --> 
    <link rel="stylesheet" href="https://js.arcgis.com/4.11/esri/themes/light/main.css" />
	  <script src="https://js.arcgis.com/4.11/"></script>

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
        "dojo/domReady!"
      ], function(Map, SceneView) {

        var map = new Map({
          basemap: "dark-gray-vector"
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

2. The JSBin `Output` panel should show a 3D view of earth that you can rotate around.


3. The Scene is flat since there is no elevation added to the SceneView.  Let's add the world elevation. 

```html 
  var map = new Map({
    basemap: "dark-gray-vector",
    ground: "world-elevation"
  });
```
Your app should look something like this:

 * [Code](index.html)
 * [Live App](https://jofraley.github.io/Hacking_JavaScript/labs/jsapi/create_starter_map_3d/index.html)

###Bonus

* Experiment with different basemaps such as `topo` or `gray`.
* Change the elevation to the topobathy service.
* Take a look at the bonus section for the [2D Starter Map](../create_starter_map/lab.md#bonus) and try the same only with the `SceneView` instead of the `MapView`.
* Run the code locally on your machine. Eventually if your app gets larger you'll want to migrate it from JSBin to your desktop.

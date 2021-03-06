###Exercise 2 Building a starter map

This lab covers the basics for creating a basic starter mapping application.
The starter map simply loads a default base map, and centers and zooms it in in a [MapView](https://developers.arcgis.com/javascript/latest/api-reference/esri-views-MapView.html).
If you are new to ArcGIS and need a full set of instructions on building a basic mapping application
visit the [Getting Started with MapView](https://developers.arcgis.com/javascript/latest/sample-code/get-started-mapview/index.html) tutorial.

1. Copy and paste the code below into a new [jsbin.com](http://jsbin.com).

  ```html
<!-- Exercise 2 , Written in JavaScript 4.11 -->
<!DOCTYPE html>
<html>
<head>
  <meta charset="utf-8">
  <meta name="viewport" content="initial-scale=1,maximum-scale=1,user-scalable=no">
  <title>2D Map</title>

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
      "esri/views/MapView",
      "dojo/domReady!"
    ], function(Map, MapView) {

      var map = new Map({
        basemap: "satellite"
      });

      var view = new MapView({
        container: "viewDiv",
        map: map,
        center: [-74.029, 40.71],
        zoom: 10
      });

    });
  </script>
</head>
<body>
  <div id="viewDiv"></div>
</body>
</html>
  ```

2. The JSBin `Output` panel should show a imagery basemap centered on New York City.

Your app should look something like this:

 * [Code](index.html)
 * [Live App](https://jofraley.github.io/Hacking_JavaScript/labs/jsapi/create_starter_map/index.html)

###Bonus

* Experiment with different basemaps such as `topo-vector` or `gray-vector`. [Map Class / Basemap Property](https://developers.arcgis.com/javascript/latest/api-reference/esri-Map.html#basemap)

* Explore the [MapView Class](https://developers.arcgis.com/javascript/latest/api-reference/esri-views-MapView.html)

* Declare the `view` variable globally instead and open your browser's javascript console ([see some instructions here for chrome](https://developers.google.com/web/tools/chrome-devtools/)). You can then interactively control the view from your browser console by referring to the `view` global variable. Many browsers will autocomplete once you've typed `view.`. For example, change the view extent, center point, zoom level or scale. See [here](https://developers.arcgis.com/javascript/latest/api-reference/esri-views-MapView.html) for some examples.

  **Hint:** If you're in a JS Bin, pop the Output into a separate window/tab to get direct access from the console.
  ``` javascript
  var view; // DECLARE the 'view' variable globally.

  require([
    "esri/Map",
    "esri/views/MapView",
    "dojo/domReady!"
  ], function(
    Map, MapView) {

    ...

    view = new MapView({ // REMOVE the 'var' so we're setting the new global 'view' variable.
      container: "viewDiv",
      map: map,
      center: [-74.029, 40.71],
      zoom: 10
    });
  ```
  Try changing the map's basemap by drilling down through the `view.map` property. E.g. `view.map.basemap = "streets"`.

  **Note:** You typically don't want to declare global variables like we do here, but it's good to know how to do it for debugging and exploring the API. Plus you're learning about JavaScript variable scope!
* Run the code locally on your machine. Eventually if your app gets larger you'll want to migrate it from JSBin to your desktop.

###Build a starter map (3.18 JS API)

This lab covers the basics for creating a basic starter mapping application with the 3.18 JS API.
The starter map simply loads a default base map, centers and zooms it in.

1. Copy and paste the code below into a new [jsbin.com](http://jsbin.com).

  ```html
  <!DOCTYPE html>
  <html>
  <head>
    <title>JS API Starter App</title>
    <meta charset=utf-8">
    <meta name="viewport" content="initial-scale=1, maximum-scale=1, user-scalable=no">
    <link rel="stylesheet" type="text/css" href="//js.arcgis.com/3.18/esri/css/esri.css">
    <script src="//js.arcgis.com/3.18/"></script>
    <style>
      html,body,#mapDiv {
        padding:0;
        margin:0;
        height:100%;
      }
    </style>
    <script>
      var map;
      require(["esri/map",
               "dojo/domReady!"],
        function(Map) {
          map = new Map("mapDiv", {
            center: [-77.029, 38.89],
            zoom: 10,
            basemap: "dark-gray"
          });
        }
      );
    </script>
  </head>

  <body>
    <div id="mapDiv"></div>
  </body>
  </html>
  ```

2. The JSBin `Output` panel should show a dark-gray map centered on Washington DC.

Your app should look something like this:
 * [Code](index.html)
 * [Live App](http://jofraley.github.io/Hacking_JavaScript/labs/jsapi3/create_starter_map/index.html)

###Make the following changes

* Experiment with different basemaps such as `streets` or `gray` (raster basemaps).  What are some of the other options for basemap names?
* Try using vector basemaps
* Change the zoom value, Higher number does what?  Lower number does what?
* Go to the [API reference](https://developers.arcgis.com/javascript/3/jsapi/map-amd.html) and look at other properties and methods available


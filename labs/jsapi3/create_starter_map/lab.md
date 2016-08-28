###Build a starter map (3.17 JS API)

This lab covers the basics for creating a basic starter mapping application with the 3.17 JS API.
The starter map simply loads a default base map, centers and zooms it in.

1. Copy and paste the code below into a new [jsbin.com](http://jsbin.com).

  ```html
  <!DOCTYPE html>
  <html>
  <head>
    <title>JS API Starter App</title>
    <meta charset=utf-8">
    <meta name="viewport" content="initial-scale=1, maximum-scale=1, user-scalable=no">
    <link rel="stylesheet" type="text/css" href="//js.arcgis.com/3.15/esri/css/esri.css">
    <script src="//js.arcgis.com/3.15compact/"></script>
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
            center: [-122.68, 45.52],
            zoom: 10,
            basemap: "topo"
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

2. The JSBin `Output` panel should show a dark-grey map centered on Portland, Oregon.

Your app should look something like this:
 * [Code](index.html)
 * [Live App](http://jofraley.github.io/Hacking_JavaScript/labs/jsapi3/create_starter_map/index.html)

###Make the following changes

* Experiment with different basemaps such as `streets` or `gray`.  What are some of the other options for basemap names?
* Try using a vector basemap
* Change the zoom value, Higher number does what?  Lower number does what?
* Commit out the line referencing the esri.css.  What happens?
* Go to the [API reference](https://developers.arcgis.com/javascript/3/jsapi/map-amd.html) and look at other properties and methods available


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
 * [Live App](http://esri.github.io/geodev-hackerlabs/develop/jsapi3/create_starter_map/index.html)

###Bonus

* Experiment with different basemaps such as `topo` or `gray`.
* Run the code locally on your machine. Eventually if your app gets larger you'll want to migrate it from JSBin to your desktop.

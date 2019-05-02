###Exercise 5, Add a Scene Layer to a 3D map

This lab covers the basics of adding a scene layer to your application.

To learn more about what options you have with SceneLayers look at the [SceneLayer API Reference](https://developers.arcgis.com/javascript/latest/api-reference/esri-layers-SceneLayer.html)

1. Click [create_starter_map_3d/index.html](../create_starter_map_3d/index.html) and copy the contents to a new [jsbin.com](http://jsbin.com).

2. In `JSBin` > `HTML`, update the `require` statement and `function` definition: 

  ```javascript 
      require([
        "esri/Map",
        "esri/views/SceneView",
        "esri/layers/SceneLayer",
        "dojo/domReady!"
      ], function(Map, SceneView, SceneLayer) {
   ```

3. Now let's add the buildings scene layer to the map:

  ```javascript
	var buildingsSL = new SceneLayer({
          url: "https://tiles.arcgis.com/tiles/0p6i4J6xhQas4Unf/arcgis/rest/services/New_York_City_3D_Buildings_Optimized/SceneServer"
        });

        var map = new Map({
          basemap: "dark-gray-vector",
          ground: "world-elevation",
          layers: [buildingsSL]
        });
  ```

2. The JSBin `Output` panel should show a 3D view of earth that you can rotate around and zoom in on Manhatten to see the 3D buildings.


3. Since we are focused on New York City, modify the SceneView to be zoomed in more and tilt the scene since we are looking at 3D data. Take a look at the SceneView and the [Camera](https://developers.arcgis.com/javascript/latest/api-reference/esri-views-SceneView.html#camera)

```javascript 
        var view = new SceneView({
          container: "viewDiv",
          map: map,
          camera:{
            position: [-74.022468, 40.699343, 790],
            tilt: 71.47,
            heading: 30.25
          }
        });
```
Your app should look something like this:

 * [Code](https://github.com/jofraley/Hacking_JavaScript/blob/master/labs/jsapi/add_scene_layer/js411_addscenelayer.html)
 * [Live App](https://jofraley.github.io/Hacking_JavaScript/labs/jsapi/add_scene_layer/js411_addscenelayer.html)

###Bonus

* Add the same layers you did in the 2D map, [MetroLines](https://services7.arcgis.com/kHi1Eco9RJ4lZsrC/arcgis/rest/services/NYC_Subway_Routes/FeatureServer/0), [BlockGroups](http://services7.arcgis.com/kHi1Eco9RJ4lZsrC/ArcGIS/rest/services/NeighborhoodTabulationAreas_NYC_2010Census_Brooklyn_Queens/FeatureServer/0), and [MetroStations](https://services1.arcgis.com/JPUKRee8mEBfJ0K4/ArcGIS/rest/services/NYC_Subway_Stations_2015/FeatureServer/0)
You can see the resulting code [here](https://github.com/jofraley/Hacking_JavaScript/blob/master/labs/jsapi/add_scene_layer_feature_layers/js411_addscenefeaturelayers.html)

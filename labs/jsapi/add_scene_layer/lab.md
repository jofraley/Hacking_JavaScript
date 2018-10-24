###Add a Scene Layer to a 3D map

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
			url: "https://tiles.arcgis.com/tiles/hRUr1F8lE8Jq2uJo/arcgis/rest/services/BuildingsDC/SceneServer/layers/0"
		});
		
        var map = new Map({
          basemap: "dark-gray-vector",
	  layers: [buildingsSL],
          ground: "world-elevation"
        });
  ```

2. The JSBin `Output` panel should show a 3D view of earth that you can rotate around and zoom in on DC to see the 3D buildings.


3. Since we are focused on DC, modify the SceneView to be zoomed in more and tilt the scene since we are looking at 3D data. Take a look at the SceneView and the [Camera](https://developers.arcgis.com/javascript/latest/api-reference/esri-views-SceneView.html#camera

```javascript 
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
You can see the resulting code [here](https://github.com/jofraley/Hacking_JavaScript/blob/master/labs/jsapi/add_scene_layer_feature_layers/index.html)

###Add a feature layer to a map

In this lab you will add a feature layer to an ArcGIS API for JavaScript application. 

* Explore the [FeatureLayer Class and its corresponding properties](https://developers.arcgis.com/javascript/latest/api-reference/esri-layers-FeatureLayer.html)
* Explore the [SceneView Class and its corresponding properties](https://developers.arcgis.com/javascript/latest/api-reference/esri-views-SceneView.html)
* Explore the [Map Class and its corresponding properties](https://developers.arcgis.com/javascript/latest/api-reference/esri-Map.html)




1. Click [create_starter_map/index.html](../create_starter_map/index.html) and copy the contents to a new [jsbin.com](http://jsbin.com).

2. In `JSBin` > `HTML`, update the `require` statement and `function` definition:

  ```javascript
  require([
    "esri/Map",
    "esri/views/MapView",
    /*** ADD ***/
    "esri/layers/FeatureLayer",
    "dojo/domReady!"
  ], function(Map, MapView, FeatureLayer) {
  ```

3. Now add the NYC Metro Lines to the map by assigning the REST endpoint url of the layer. 

  ```javascript
    ...

    /*** ADD ***/

    var metrolines = new FeatureLayer({
      url: "https://services.arcgis.com/bGgB6gXiQ835YdNp/arcgis/rest/services/NYC_SubwayLines/FeatureServer/1"
    });

    map.add(metrolines);
  ```

4. Confirm that the JSBin `Output` panel shows a map with rail lines.

Your app should look something like this:
* [Code](index.html)
* [Live App](http://jofraley.github.io/Hacking_JavaScript/labs/jsapi/add_feature_layer/index.html)

5. 
###Bonus
* Add a [Metro Stations feature layer](https://services.arcgis.com/bGgB6gXiQ835YdNp/arcgis/rest/services/NYC_SubwayStations/FeatureServer/0) to the map,
 and then add a [BlockGroups feature layer](https://services.arcgis.com/bGgB6gXiQ835YdNp/arcgis/rest/services/NYC_Neighborhoods/FeatureServer/2).
* Ensure the layers are ordered with polygons on the bottom, lines and then points on top.
* If you added additional layers using the `add()` method, try the `addMany()` method instead [code for addMany](https://github.com/jofraley/Hacking_JavaScript/blob/master/labs/jsapi/add_feature_layer_constructor/add_featurelayers.html). Here is [example code](https://github.com/jofraley/Hacking_JavaScript/blob/master/labs/jsapi/add_feature_layer_constructor/addmany.html) of adding layers in the constructor for the map.  Read up on the `layers` collection and see how the API gives you a few ways to [add layers to a map](https://developers.arcgis.com/javascript/latest/api-reference/esri-Map.html#layers).
*'map.addMany([nyc_blocks, metrolines, stations]);'
* Watch for a property change on a feature layer loading.  Check the API for examples of [how to watch for property changes.](https://developers.arcgis.com/javascript/latest/guide/working-with-props/index.html)  In the jsbin.com you will want to open the console so you can see the output.  [Here is the example code.](../add_feature_layer_watchproperty/index.html)
```html
  metroStationsFL.watch("loadStatus", function(status) {
        // status types not-loaded, loading, loaded, failed
        console.log("'" + metroStationsFL.title + "'" + " " + status);
        if (status === "failed") {
            console.log(poi.loadError);
  }
```
* The 4.x JS API works closely with ArcGIS Portals. Instead of the Feature Service URL, [use the Portal Item ID](https://developers.arcgis.com/javascript/latest/api-reference/esri-layers-FeatureLayer.html#portalItem) to add layers. See the URLs of the Portal Items for [Stations](http://esrifederal.maps.arcgis.com/home/item.html?id=d2800734a998448f9c4dc81014c52905), [Lines](http://esrifederal.maps.arcgis.com/home/item.html?id=61b9ea7e364040389ff2f205d0151b74) and [BlockGroups](http://esrifederal.maps.arcgis.com/home/item.html?id=baac7d7be5a846388ec64252d9bdd6ca) to get their IDs.  [Here is the code.](../add_feature_layer_portalitems/index.html).  What functionality do you gain in the app automatically when using the Portal Id's vs adding the feature layer using the url?

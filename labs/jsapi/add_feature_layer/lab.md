###Add a feature layer to a map

In this lab you will add a feature layer to an ArcGIS API for JavaScript application. 

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

3. Now add the Metro Lines to the map:

  ```javascript
    ...

    /*** ADD ***/

    var featureLayer = new FeatureLayer({
      url: "https://services.arcgis.com/lA2FZKuu26Fips7U/ArcGIS/rest/services/MetroLines/FeatureServer/0"
    });

    map.add(featureLayer);
  ```

4. Confirm that the JSBin `Output` panel shows a map with rail lines.

Your app should look something like this:
* [Code](index.html)
* [Live App](http://jofraley.github.io/Hacking_JavaScript/labs/jsapi/add_feature_layer/index.html)

###Bonus
* Add a [Metro Stops feature layer](https://services.arcgis.com/lA2FZKuu26Fips7U/arcgis/rest/services/MetroStops/FeatureServer/0) to the map,
 and then add a [BlockGroups feature layer](http://services.arcgis.com/lA2FZKuu26Fips7U/arcgis/rest/services/BlockGroupsDC/FeatureServer/0).
* Ensure the layers are ordered with polygons on the bottom, lines and then points on top.
* If you added additional layers using the `add()` method, try the `addMany()` method instead [code for addMany](../addmany_feature_layer/index.html). Here is [example code](../add_feature_layer_constructor/index.html) of adding layers in the constructor for the map.  Read up on the `layers` collection and see how the API gives you a few ways to [add layers to a map](https://developers.arcgis.com/javascript/latest/api-reference/esri-Map.html#layers).
* Watch for a property change on a feature layer loading.  Check the API for examples of [how to watch for property changes.](https://developers.arcgis.com/javascript/latest/guide/working-with-props/index.html)  In the jsbin.com you will want to open the console so you can see the output.  [Here is the example code.](../add_feature_layer_watchproperty/index.html)
* The 4.0 JS API works closely with ArcGIS Portals. Instead of the Feature Service URL, [use the Portal Item ID](https://developers.arcgis.com/javascript/latest/api-reference/esri-layers-FeatureLayer.html#portalItem) to add layers. See the URLs of the Portal Items for [Stops](http://www.arcgis.com/home/item.html?id=74f55caeacc24e77bcceee84f5f23ed1), [Lines](http://www.arcgis.com/home/item.html?id=a9bb0518e0154eb58695b4ad76f14201) and [BlockGroups](http://www.arcgis.com/home/item.html?id=300c8eb592a34ffea3862fe540f9e9b2) to get their IDs.

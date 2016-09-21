###Create an App from a Web Map

You can use the [ArcGIS API for JavaScript](https://developers.arcgis.com/javascript/) to easily load web maps built with the Map Viewer. The advantage to using this approach is that the map will contain all of the pre-defined settings you configured in the Map Viewer. e.g. Layer Styles, Popups, Refresh Rate.

In this lab, you will use the ArcGIS JS API to load a WebMap by its ID in a custom JavaScript app. 

1. Click [JS API starter map HTML](../../jsapi/create_starter_map/index.html) and copy the contents to a new [jsbin.com](http://jsbin.com).

2. In `JSBin` > `HTML`, update the `require` statement and `function` definition (notice to remove the `Map` reference).

	```javascript
  require([
    "esri/views/SceneView",
    /*** ADD ***/
    "esri/WebScene",
    "dojo/domReady!"
  ], function(SceneView, WebScene) {
	```

3. Open a WebMap into the MapView using the WebMapID. Note that the code is really similar to the starter map, but in place of a Map we're using a WebMap. To use the saved initial view of the WebMap, remove the `center` and `zoom` attributes from the `MapView` options.
	
  NOTE: Feel free to use your own WebMap ID below!

	```javascript
  require([
    "esri/views/SceneView",
    "esri/WebScene",
    "dojo/domReady!"
  ], function(SceneView, WebScene) {

    /*** REPLACE ***/

    var scene = new WebMap({
      portalItem: { // autocasts as new PortalItem
        id: "6b06eb3e7db04dfab1b67d49e654c587"
      }
    });

    var view = new SceneView({
      map: scene,
      container: "viewDiv"
    });
	```

Your app should look something like this:
 * [Code](index.html)
 * [Live App](http://jofraley.github.io/Hacking_JavaScript/labs/webmap_apps/create_jsapi_scene_app/index.html)


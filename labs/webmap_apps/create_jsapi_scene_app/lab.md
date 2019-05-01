###Create an 3D Application from a 3d Web Scene

You can use the [ArcGIS API for JavaScript](https://developers.arcgis.com/javascript/) to easily load web scenes built with the Scene Viewer. The advantage to using this approach is that the map will contain all of the pre-defined settings you configured in the Scene Viewer. 

In this lab, you will use the ArcGIS JS API to load a Scene by its ID in a custom JavaScript app. 

1. Look through the [WebScene Class Documentation](https://developers.arcgis.com/javascript/latest/api-reference/esri-WebScene.html)

1. Click [JS API starter 3D app HTML](../../jsapi/create_starter_map_3d/index.html) and copy the contents to a new [jsbin.com](http://jsbin.com).

2. In `JSBin` > `HTML`, update the `require` statement and `function` definition (notice we add the WebScene reference).

```javascript
  require([
    "esri/views/SceneView",
    /*** ADD ***/
    "esri/WebScene",
    "dojo/domReady!"
  ], function(SceneView, WebScene) {
```

3. Open a Web Scene into the SceneView using the WebSceneID. Note that the code is really similar to the starter map, but in place of a Map we're using a WebScene. To use the saved initial view of the WebScene, remove the `center` and `zoom` attributes from the `MapView` options.  Also add the portalItem id to reference the Web Scene.
	
  NOTE: Feel free to use your own Web Scene ID below!

```javascript
  require([
    "esri/views/SceneView",
    "esri/WebScene",
    "dojo/domReady!"
  ], function(SceneView, WebScene) {

    /*** REPLACE ***/

    var scene = new WebScene({
      portalItem: { // autocasts as new PortalItem
        id: "611e614e01e2459785b9a27b9c73313a"
      }
    });

    var view = new SceneView({
      map: scene,
      container: "viewDiv"
    });
```
Here are some additional scenes that you can try out: 
* 31ea5dd6b2e74a1aaf3b0619a03b53c1
* a49ce6d0705b41fb81b6dea098a8b2f7


Your app should look something like this:
 * [Code](https://github.com/jofraley/Hacking_JavaScript/blob/master/labs/webmap_apps/create_jsapi_scene_app/js411_scene.html)
 * [Live App](http://jofraley.github.io/Hacking_JavaScript/labs/webmap_apps/create_jsapi_scene_app/index.html)



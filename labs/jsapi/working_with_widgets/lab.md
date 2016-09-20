###Search with Widget

In this lab you will add a search widget and search against a feature layer. The widget performs context-sensitve search as you type and then it will zoom to and highlight the feature selected. You can also format the data in the popup that appears. 

In this lab it will search against the neighborhood polygon layer but you can point to any hosted feature layer you want.

1. Click [create_starter_map/index.html](../create_starter_map/index.html) and copy the contents to a new [jsbin.com](http://jsbin.com).

2. In `JSBin` > `HTML`, update the `require` statement and `function` definition:

  ```javascript
  require([
    "esri/Map",
    "esri/views/MapView",
    /*** ADD ***/
    "esri/widgets/Search",
    "esri/widgets/BasemapToggle",
    "dojo/domReady!"
  ], function(Map, MapView, Search, BasemapToggle) { /*** ADD ***/ 
  ```

3. Create the Search widget and add it to the `top-right` of the UI.

  ```javascript
    ...      
    var map = new Map({
        basemap: "dark-gray"
    });

    var view = new MapView({
      container: "viewDiv",
      map: map,
      center: [-77.029, 38.89], // lon, lat
      zoom: 10
    });

    /*** ADD ***/

    // Create search widget
   var search = new Search({
        view: view
   });

    // Add widget to the UI
    view.ui.add(searchWidget, "top-right");
 
  ```

  At this point, the map will allow you to search against the default ArcGIS Online Geocoding Service. Give it a go. You can enter an address or point of interest (like `Providence Park` or `DCA`) or a geography (like `Tysons Corner` or `USA`).

4. Now add the BasemapToggle widget. First add the correct module and variable name.

  ```javascript
   require([
      "dojo/dom-construct",
      "esri/Map",
      "esri/views/MapView",
      "esri/widgets/Search",
	  "esri/widgets/BasemapToggle",
      "dojo/domReady!"
    ], function(domConstruct, Map, MapView, Search, BasemapToggle) {
    ...
    
    var toggle = new BasemapToggle({
        view: view,
        nextBasemap: "hybrid"
    });

    //psotion on UI
    view.ui.add(toggle, "top-left");

    
  ``.

Your app should look something like this:
* [Code](index.html)
* [Live App](http://jofraley.github.io/Hacking_JavaScript/labs/jsapi/working_with_widgets/index.html)

###Bonus
* Play around with positions on UI
* Add a logo to the bottom right of the UI

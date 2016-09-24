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
    "esri/layers/FeatureLayer",
    "dojo/domReady!"
  ], function(Map, MapView, Search, FeatureLayer) { /*** ADD ***/ 
  ```

3. Create the Search widget and add it to the `top-right` of the UI.

  ```javascript
    ...      

    var view = new MapView({
      container: "viewDiv",
      map: map,
      center: [-77.029, 38.89], // lon, lat
      zoom: 10
    });

    /*** ADD ***/

    // Create search widget
    var searchWidget = new Search({
      view: view,
      allPlaceholder: "Metro Stop e.g. Dupont Circle"
    });

    // Initialize the widget
    searchWidget.startup();

    // Add widget to the UI
    view.ui.add(searchWidget, {
      position: "top-right"
    });    
  ```

  At this point, the map will allow you to search against the default ArcGIS Online Geocoding Service. Give it a go. You can enter an address or point of interest (like `Washtington Monument` or `DCA`) or a geography (like `Tysons Corner` or `USA`).

4. Now add the Metro Stops Feature Service as a search source to the widget. This will allow you to search for different metro stops by the `NAME` field. Also notice that a template is added for the popup to format the data nicely.

  ```javascript
    ...
    
    var searchWidget = new Search({
      view: view,
      allPlaceholder: "Metro Stop e.g. Dupont Circle"
    });

    /*** ADD ***/

    var sources = [];
    
    // Add the default world geocoder source
    sources.push(searchWidget.defaultSource);

    // Add the feature layer source to search      
    sources.push({
      featureLayer: new FeatureLayer("https://services.arcgis.com/lA2FZKuu26Fips7U/ArcGIS/rest/services/MetroStops/FeatureServer/0"),
      name: "Metro Stop Search",
      searchFields: ["NAME"],
      displayField: "NAME",
      exactMatch: false,
      outFields: ["*"],
      placeholder: "Metro Stop e.g. Dupont Circle",
      // Create a PopupTemplate to format data
      popupTemplate: {
        title: "{NAME}",
        content: "Line: {LINE}</br>Address: {ADDRESS}</br>Transfer Station: {Transfer}</br><a href={WEB_URL}>More info</a>"
      }
    });

    // Set the sources
    searchWidget.sources = sources;
  ```

5. In JSBin, run the app and type in `Crystal City` or `Dupont Circle`. The app should highlight and zoom into the metro stop, and a popup should also be displayed with there field data.

Your app should look something like this:
* [Code](index.html)
* [Live App](http://jofraley.github.io/Hacking_JavaScript/labs/jsapi/search_with_widget/index.html)

###Bonus
* Add the [Metro Stops](http://services.arcgis.com/lA2FZKuu26Fips7U/ArcGIS/rest/services/MetroStops/FeatureServer/0) and [Metro Lines](http://services.arcgis.com/lA2FZKuu26Fips7U/ArcGIS/rest/services/MetroLines/FeatureServer/0) to the map.

###Style a Layer Popup

In this lab you will use code to style a popup.

1. Copy [Exercise 6](https://github.com/jofraley/Hacking_JavaScript/blob/master/labs/jsapi/style_feature_layer/js411_stylingfeatures.html) and copy the contents to a new [jsbin.com](http://jsbin.com).

2. In `JSBin` > `HTML`, update the `require` statement and `function` definition:

  ```javascript
    require([
      "esri/Map",
      "esri/views/MapView",
      "esri/layers/FeatureLayer",
      /*** ADD ***/
      "esri/symbols/SimpleLineSymbol",
      "esri/renderers/UniqueValueRenderer",
      "esri/PopupTemplate",
      "esri/widgets/Legend",
      "dojo/domReady!"
    ], function(Map, MapView, FeatureLayer, SimpleLineSymbol, UniqueValueRenderer, PopupTemplate, Legend) {
  ```

3. Now add a new PopupTemplate to the Metro Stations Feature Layer with the popup template style desired:

  ```javascript
    /*** ADD ***/
      var popupTemplate = new PopupTemplate({
        title: "Station Name: {MTA_STATIONS}",
        content: [{ 
          type: "text",     
          text: "Total 2015 Passengers: <b>{Annual_Ridership_2015_Long} </b>. <br>Busyness Ranking: <b>{F2015_Rank_Annual}</b>"
        }]
      });
  ```
4. Now add the template to its assigned feature layer and add the featurelayer to the map.

  ```javascript
      var metrolines = new FeatureLayer({
        url: "http://services7.arcgis.com/kHi1Eco9RJ4lZsrC/arcgis/rest/services/NYC_Subway_Routes/FeatureServer",
        renderer: renderer
      });


      var stations = new FeatureLayer({
        url: "https://services1.arcgis.com/JPUKRee8mEBfJ0K4/arcgis/rest/services/NYC_Subway_Stations_2015/FeatureServer",
        popupTemplate: popupTemplate
      });

      map.add(metrolines);
      map.add(stations);
  ```

5. Confirm that the JSBin `Output` panel shows styled popups when you click on the block groups.

Your app should look something like this:
* [Code](https://github.com/jofraley/Hacking_JavaScript/blob/master/labs/jsapi/style_simple_popup/js411popup.html)
* [Live App](http://jofraley.github.io/Hacking_JavaScript/labs/jsapi/style_simple_popup/js411popup.html)

Bonus
* Add a legend and make sure to add the infoDiv to the html
  ```
  var legend = new Legend({
         view: view,
         layerInfos: [
         {
           layer: blockgroups,
           title: "Total Population"
         }]
  }, "infoDiv");
* Play with the popup docking within the view.  Have the popup dock in the top-right of the view and not allow user to undock.  See [popup for more info](https://developers.arcgis.com/javascript/latest/api-reference/esri-widgets-Popup.html#dockOptions)

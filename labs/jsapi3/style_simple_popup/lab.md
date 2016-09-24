###Style a Layer Popup

In this lab you will use code to style a popup.

1. Click [create_starter_map/index.html](../create_starter_map/index.html) and copy the contents to a new [jsbin.com](http://jsbin.com).

2. In `JSBin` > `HTML`, update the `require` statement and function definition:

  ```javascript
  require(["esri/map",
          "esri/layers/FeatureLayer", 
          "esri/dijit/PopupTemplate",
          "dojo/domReady!"],
      function(Map, FeatureLayer, PopupTemplate) {
      ...
  ```

4. Now add create a new PopupTemple with the popup template style desired:

  ```javascript
  function(Map, FeatureLayer, PopupTemplate) {
    map = new Map("map", {
      center: [-77.029, 38.89],
      zoom: 12,
      basemap: "dark-gray"
    });

    var popupTemplate = new PopupTemplate({
      title: "Block Groups",
      // Fields
      fieldInfos: [
       { fieldName: "P0010001", label: "Total Population", visible: true, format: { places: 0 } },
       { fieldName: "H0010001", label: "Total Housing Units", visible: true, format: { places: 0 } },
       { fieldName: "H0010002", label: "Occupied Housing Units", visible: true, format: { places: 0 } },
       { fieldName: "H0010003", label: "Vacant Housing Units", visible: true, format: { places: 0 } }
      ],
      // Charts
     mediaInfos: [
     {
      title: "Demographics",
      type: "barchart",
      value: {
        fields: [
          "H0010001",
          "H0010002",
          "H0010003"
        ]
      }
     }]
   });
  ```
5. Now add the template to the feature layer and add the featurelayer to the map.

  ```javascript
   var featureLayer = new FeatureLayer("https://services.arcgis.com/lA2FZKuu26Fips7U/arcgis/rest/services/BlockGroupsDC/FeatureServer/0", {
      outFields: ["*"],
      infoTemplate: popupTemplate
   });
   map.addLayer(featureLayer);
  ```

6. Confirm that the JSBin `Output` panel shows styled popups when you click on the block groups.

Your app should look something like this:
* [Code](index.html)
* [Live App](https://jofraley.github.io/Hacking_JavaScript/labs/jsapi3/style_simple_popup/index.html)

Bonus
* Combine the code from the last lab with this one so the features are styled along with the popup.

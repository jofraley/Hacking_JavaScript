###Style Feature Layers| Excercise 6 , Written in JavaScript 4.11 / Styling Feature Layers in a Web Map


The NYC Subway system is split between the A Division (Numbering) and B Division (Letters). In this lab you will apply custom styling to the subway line feature layer to highlight these divisions.


* Explore the [FeatureLayer Class and its corresponding properties](https://developers.arcgis.com/javascript/latest/api-reference/esri-layers-FeatureLayer.html)
* Explore the [SimpleLineSymbol Class and its corresponding properties](https://developers.arcgis.com/javascript/latest/api-reference/esri-symbols-SimpleLineSymbol.html)
* Explore the [UniqueValueRenderer Class and its corresponding properties](https://developers.arcgis.com/javascript/latest/api-reference/esri-renderers-UniqueValueRenderer.html)


1. Click [create_starter_map/index.html](../create_starter_map/index.html) and copy the contents to a new [jsbin.com](http://jsbin.com).

2. In `JSBin` > `HTML`, update the `require` statement and `function` definition.

  ```javascript
    require([
      "esri/Map",
      "esri/views/MapView",
      "esri/layers/FeatureLayer",
      /*** ADD ***/
      "esri/symbols/SimpleLineSymbol",
      "esri/renderers/UniqueValueRenderer",
      "dojo/domReady!"
    ], function(Map, MapView, FeatureLayer, SimpleLineSymbol, UniqueValueRenderer) {
  ```

3. Now set up a `UniqueValueRenderer` based off the `Division` field. You can check out the list of fields by navigating to the service endpoint

  ```javascript
    ...

    var view = new MapView({
        container: "viewDiv",
        map: map,
        center: [-73.99, 40.71],
        zoom: 12
      });

      //Add this   
     var renderer = new UniqueValueRenderer({
       field: "Division", 
       defaultSymbol: new SimpleLineSymbol()
     });
  ```

4. Next we tell the renderer how to represent each discrete `Division` value. We will define the line color / properties for each division of the NYC Subway system.  
  ```javascript
    var renderer = new UniqueValueRenderer({
        field: "Division", 
        defaultSymbol: new SimpleLineSymbol()
      });

      renderer.addUniqueValueInfo("IRT",
        new SimpleLineSymbol({
          color: "blue",
          width: "2px",
          style: "solid",
        })
      );

      renderer.addUniqueValueInfo("BMT",
        new SimpleLineSymbol({
          color: "green",
          width: "2px",
          style: "solid",
        })
      );

      renderer.addUniqueValueInfo("IND",
        new SimpleLineSymbol({
          color: "green",
          width: "2px",
          style: "solid",
        })
      );
  ```

5. Lastly, we create the `FeatureLayer`(NYC Subway lines), attach the `UniqueValueRenderer`, and add it to the map.

  ```javascript
    /*** ADD ***/
    var metrolines = new FeatureLayer({
        url: "http://services7.arcgis.com/kHi1Eco9RJ4lZsrC/arcgis/rest/services/NYC_Subway_Routes/FeatureServer/1",
        renderer: renderer
     });

    map.add(metrolines);
  ```

Your app should look something like this:
 * [Code](https://github.com/jofraley/Hacking_JavaScript/blob/master/labs/jsapi/style_feature_layer/js411_stylingfeatures.html)
 * [Live App](http://jofraley.github.io/Hacking_JavaScript/labs/jsapi/style_feature_layer/js411_stylingfeatures.html)



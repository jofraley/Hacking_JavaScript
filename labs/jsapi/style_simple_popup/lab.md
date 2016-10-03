###Style a Layer Popup

In this lab you will use code to style a popup.

1. Click [create_starter_map/index.html](../create_starter_map/index.html) and copy the contents to a new [jsbin.com](http://jsbin.com).

2. In `JSBin` > `HTML`, update the `require` statement and `function` definition:

  ```javascript
  require([
    "esri/Map",
    "esri/views/MapView",
    /*** ADD ***/
    "esri/layers/FeatureLayer",
    "esri/PopupTemplate",
    "esri/renderers/ClassBreaksRenderer",
	  "esri/symbols/SimpleFillSymbol",
	  "esri/widgets/Legend",
    "dojo/domReady!"
  ], function(Map, MapView, FeatureLayer, PopupTemplate, ClassBreaksRenderer, SimpleFillSymbol, Legend) {
  ```
3. reate the map and define the symbols and ClassBreaksRenderer for the Block Groups
  ```javascript
   ...
  var map = new Map({
         basemap: "dark-gray"
  });
	  
	var view = new MapView({
         container: "viewDiv",
         map: map,
         center: [-77.029, 38.89],
         zoom: 10
  });
       
//Define symbols for each class break.
	  var less500 = new SimpleFillSymbol({
	    color: "#7EAAFF",
		style: "solid",
		outline: {
		  width: 0.5,
		  color:  "white"
		}
	  });
	  
	  var less1500 = new SimpleFillSymbol({
	    color: "#6591FF",
		style: "solid",
		outline: {
		  width: 0.5,
		  color:  "white"
		}
	  });
	  
	  var less2500 = new SimpleFillSymbol({
	    color: "#4B77F2",
		style: "solid",
		outline: {
		  width: 0.5,
		  color:  "white"
		}
	  });
	  
	  var more3500 = new SimpleFillSymbol({
	    color: "#325ED9",
		style: "solid",
		outline: {
		  width: 0.5,
		  color:  "white"
		}
	  });
	  
	  var less3500 = new SimpleFillSymbol({
	    color: "#1844BF",
		style: "solid",
		outline: {
		  width: 0.5,
		  color:  "white"
		}
	  });
	  
	  //Define the ClassBreaksRenderer
	   var renderer = new ClassBreaksRenderer({
        field: "P0010001",
        classBreakInfos: [
        {
          minValue: 0,
          maxValue: 499,
          symbol: less500,
          label: "< 500"
        }, {
          minValue: 500,
          maxValue: 1499,
          symbol: less1500,
          label: "500 - 1499"
        }, {
          minValue: 1500,
          maxValue: 2499,
          symbol: less2500,
          label: "1500 - 2499"
        }, {
          minValue: 2500,
          maxValue: 3499,
          symbol: less3500,
          label: "2500 - 3499"
        }, {
		  minValue: 3500,
          maxValue: 10000,
          symbol: more3500,
          label: "<= 3500"
        }]
      });
      
4. Now add create a new PopupTemple with the popup template style desired:

  ```javascript
    ...

    /*** ADD ***/

    var popupTemplate = new PopupTemplate({
		  title: "Total Population is {P0010001}",
		  content: [{ 
            type: "text",		  
			text: "Total Housing Units: {H0010001}.  {H0010002} are occupied and {H0010003} are vacant."
		  	},{
        	type: "media",
            mediaInfos: [{
              title: "<b>Housing Units</b>",
              type: "pie-chart",
              value: {
                theme: "BlueDusk",
                fields: [
                  "H0010002",
				  "H0010003"
                ],
              }
            }]
          }]
		});
  ```
5. Now add the template to the feature layer and add the featurelayer to the map.

  ```javascript
    var blockgroups = new FeatureLayer({
        url: "https://services.arcgis.com/lA2FZKuu26Fips7U/arcgis/rest/services/BlockGroupsDC/FeatureServer/0",
        outFields: ["*"],
		renderer: renderer,
        popupTemplate: popupTemplate
	   });
	  
	   map.add(blockgroups);
  ```

6. Confirm that the JSBin `Output` panel shows styled popups when you click on the block groups.

Your app should look something like this:
* [Code](index.html)
* [Live App](http://jofraley.github.io/Hacking_JavaScript/labs/jsapi/style_simple_popup/index.html)

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
* Play with the popup docking within the view.  Have the popup dock in the bottom-right of the view and not allow user to undock.  See [popup for more info](https://developers.arcgis.com/javascript/latest/api-reference/esri-widgets-Popup.html#dockOptions)

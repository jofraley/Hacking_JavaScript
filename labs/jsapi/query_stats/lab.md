###Search with a Query Task

In this lab you will use a QueryTask to query data from a feature layer. A query task allows you to make a SQL or spatial query to retrieve subsets of records from a layer or service. With query tasks, you are responsible for adding the resulting features to the map as graphics. You are also responsible for adding a symbol and the defining the popup template used for the data.

1. Click [create_starter_map/index.html](../create_starter_map/index.html) and copy the contents to a new [jsbin.com](http://jsbin.com).

2. In `JSBin` > `HTML`, add the query select menu `div` element and the sql query options:

  ```html
    <body>
      <div id="viewDiv"></div>
      <!-- ADD -->
      <select id="queryDiv">
        <option selected>blue</option>
        <option>orange</option>
        <option>red</option>
	      <option>silver</option>
        <option>yellow</option>
        <option>green</option>
      </select>
    </body>
  ```

3. Update the `require` statement and `function` definition:

  ```javascript
  require([
    "esri/Map",
    "esri/views/MapView",
    /*** ADD ***/
    "esri/layers/FeatureLayer",
    "esri/tasks/QueryTask",
    "esri/tasks/support/Query",
    "esri/symbols/SimpleMarkerSymbol",
    "esri/core/watchUtils",
    "dojo/on",
    "dojo/dom",
    "dojo/domReady!"
  ], function(Map, MapView, FeatureLayer, QueryTask, Query, SimpleMarkerSymbol, watchUtils, on, dom) { /*** ADD ***/
  ```

4. Now add the `QueryTask` and `Query` to select features from the metro stops layer. Only return two fields from the layer.

  ```javascript
    ...

    var view = new MapView({
      container: "viewDiv",
      map: map,
      center: [-77.029, 38.89], // lon, lat
      zoom: 10
    });

    /*** ADD ***/

    // Create query task to reference the PDX_Rail_Stops_Styled feature layer      
    var queryTask = new QueryTask({
      url: "https://services.arcgis.com/lA2FZKuu26Fips7U/ArcGIS/rest/services/MetroStops/FeatureServer/0"
    });

    // Only return three fields
    var query = new Query({
      returnGeometry: true,
      outFields: ["NAME", "LINE"],
      where: ""
    });
  ```

5. Next, add functions to execute the query task, get features, and then add them to the default graphics layer of the view. Notice that features will not be added until the view promise is ready.

  ```javascript
    /*** ADD ***/

    // Perform query when page loads
    getFeatures("blue");
	  
      // Get features with query where clause
      function getFeatures(theColor) {
        query.where = "LINE LIKE '%" + theColor + "%'";
        queryTask.execute(query).then(function(results) {
			    if (!view.ready) {
			      watchUtils.whenTrueOnce(view, "ready", function() {
                addFeatures(results.features);
			      })
			    } else {
			      addFeatures(results.features);
			    }  
        })
		    .otherwise(promiseRejected);
      }

    // Add features as graphics
      function addFeatures(features) {
        var color,
          symbol;
        // Color
        switch (dom.byId("queryDiv").value) {
          case "blue":
            color = [0, 0, 255]
            break;
          case "red":
            color = [255, 0, 0]
            break;
          case "green":
            color = [0, 255, 0]
            break;
		  case "orange":
            color = [255,165, 0]
            break;	
		  case "yellow":
            color = [255, 255, 0]
            break;
		  case "silver":
            color = [190, 190, 190]
            break;
        }
        symbol = new SimpleMarkerSymbol({
          color: color,
          size: 8,
          outline: {
            color: [ 255, 255, 255 ],
            width: 1
          }
        });
      // Set symbol and popup template
        for (var i=0; i < features.length; i++) {
          var feature = features[i];
          feature.symbol = symbol;
          feature.popupTemplate = {
            title: "{NAME}",
            content: "This is a metro stop for the {LINE} line(s)."
          }
          view.graphics.add(feature);
        }
        // Add graphics
        view.graphics.removeAll();
        view.graphics.addMany(features);
        view.goTo(features);
        view.popup.visible = false;
      }
  ```

6. Finally, add the select HTML element and an event handler so we can change the SQL string for the query.

  ```javascript
    /*** ADD ***/

    // Add select element to UI
    view.ui.add(dom.byId("queryDiv"), {
      position: "top-right"
    });

   // Get the color from the drop down
    on(dom.byId("queryDiv"), "change", function(e) {
	    var theColor = e.target.value;
		  getFeatures(theColor);
	  })
 
    //Add the promise rejected function
    function promiseRejected(){
	    console.write("Error with query");
	  }

  ```

7. In JSBin, run the app > Select a category to query the feature layer. Click on any point to display the attribute data in a popup.

Your app should look something like this:
* [Code](index.html)
* [Live App](http://jofraley.github.io/Hacking_JavaScript/labs/jsapi/query_stats/index.html)

###Query by distance with statistics

In this lab you will use a query with distance and then clone that query to do stats on the features selected. 

1. Click [create_starter_map/index.html](../create_starter_map/index.html) and copy the contents to a new [jsbin.com](http://jsbin.com).

2. In `JSBin` > `HTML`, first let's add some css for how we want our statistics values to be display with color and font size and weight:

  ```css
     html,
    body, #viewDiv {
      padding: 0;
      margin: 0;
      height: 100%;
    }
    /***ADD***/
    #tot-crimes {
      color: #ed5050;
      font-size: 36pt;
      font-weight: bolder;
      line-height: 0.8;
    }

    #tot-Homicides,#tot-theft,#tot-Arson,
    #tot-theftAuto,#tot-MVT,#tot-Robbery,
    #tot-AssaultwWeapon,#tot-Burglary,#tot-SexAbuse{
      color: #149dcf;
      font-size: 20pt;
      font-weight: bolder;
      line-height: 0.8;
    }
  ```

3. Update the `require` statement and `function` definition:

  ```javascript
 require([
      "esri/views/MapView",
      "esri/WebMap",
      "esri/widgets/Legend",
      "esri/widgets/Expand",
      "esri/core/lang",
      "esri/core/promiseUtils",
      "esri/core/watchUtils",
      "dojo/domReady!"
    ], function(
      MapView, WebMap, Legend, Expand, lang, promiseUtils,watchUtils, domReady) {
  ```

4. Let's use the portal id for a new web map that contains crime data and use this webmap in the view.  Notice that we are setting some addtional properties on the view like the [contraints](https://developers.arcgis.com/javascript/latest/api-reference/esri-views-MapView.html#constraints) of minScale and [highlightOptions](https://developers.arcgis.com/javascript/latest/api-reference/esri-views-MapView.html#highlightOptions).  Look at the links to get more info and play around with some different properties as you build out this lab.

  ```javascript
          var webmap = new WebMap({
        portalItem: {
          id: "ac967bafb27d4ded896975e18e928fca"
        }
      });

      var view = new MapView({
        map: webmap,
        container: "viewDiv",
        constraints: {
          minScale: 300000
        },
        highlightOptions: {
          color: "yellow",
          haloOpacity: 0.65,
          fillOpacity: 0.45
        }
      });
```

5. Next, we are going to add a UI element to show the results of our query.  Notice that the values in the span id='', the id's are the same as you defined in your css above.

  ```javascript
    var titleContent = document.createElement("div");
      titleContent.style.padding = "15px";
      titleContent.style.backgroundColor = "white";
      titleContent.style.width = "500px";
      titleContent.innerHTML = [
        "<div id='title' class='esri-widget'>",
        "<span id='tot-crimes'>0</span> crimes occurred within half mile of the pointer location in 2018.<br>",
        "Total number of Theft are <span id='tot-theft'>0</span>.<br>",
		"Total number of Theft from Auto are <span id='tot-theftAuto'>0</span>.<br>",
		"Total number of Motor Vehicle Theft are <span id='tot-MVT'>0</span>.<br>",
		"Total number of Robberies are <span id='tot-Robbery'>0</span>.<br>",
		"Total number of Assault with a Dangerous Weapon are <span id='tot-AssaultwWeapon'>0</span>.<br>",
		"Total number of Burglaries <span id='tot-Burglary'>0</span>.<br>",
		"Total number of Sex Abuse are <span id='tot-SexAbuse'>0</span>.<br>",
		"Total number of Homicides are <span id='tot-Homicides'>0</span>.<br>",
		"Total number of Arson are <span id='tot-Arson'>0</span>.<br>",
        "</div>"
      ].join(" ");

      var titleExpand = new Expand({
        expandIconClass: "esri-icon-dashboard",
        expandTooltip: "Summary stats",
        view: view,
        content: titleContent,
        expanded: view.widthBreakpoint !== "xsmall"
      });
      view.ui.add(titleExpand, "top-right");
  ```

6. Next let's add a legend which should be easy for you to do.  Here we will add it into a Expand widget as well like we did above.  Notice that both the above and legend are using the [widthBreakpoint property](https://developers.arcgis.com/javascript/latest/api-reference/esri-views-MapView.html#widthBreakpoint) to dynamically control the expansion of the widget.  You can see this by making the browser window smaller.  

  ```javascript
          var legendExpand = new Expand({
        view: view,
        content: new Legend({
          view: view
        }),
        expanded: view.widthBreakpoint !== "xsmall"
      });
      view.ui.add(legendExpand, "bottom-left");

      view.watch("widthBreakpoint", function(newValue) {
        titleExpand.expanded = newValue !== "xsmall";
        legendExpand.expanded = newValue !== "xsmall";
      });
  ```
7. You need to declare a variable so that we can control what is highlighted as the selection is changed.  This will be used later.
```javascript
	var highlightHandle = null;
```
8. Now we need to right the code when to start doing the query. What this is saying is that when the view is ready, then we get the first layer in the map which is our crime points.  So when the view is not updating once [watchUtils.whenFalseOnce](https://developers.arcgis.com/javascript/latest/api-reference/esri-core-watchUtils.html#whenFalseOnce) when there is a click or drag on the view, we capture the location and stop navigations using event.stopPropagation() so as we drag it doesn't pan the map. But instead we call the function to queryStatsOnDrag and pass the layerView and the event(the location clicked).
```javascript
      view.when()
        .then(function() {
          var layer = webmap.layers.getItemAt(0);
          view.whenLayerView(layer).then(function(layerView) {
            watchUtils.whenFalseOnce(layerView, "updating", function(
              val) {
              // Query layer view statistics as the user clicks
              // or drags the pointer across the view.
              view.on(["click", "drag"], function(event) {
                // disables navigation by pointer drag
                event.stopPropagation();
                queryStatsOnDrag(layerView, event)
                  .then(updateStats);
              });
            });
          });
        });
```
9.  What does the queryStatsOnDrag function look like.  It looks complicated but it really isn't.  First we create a query on the layerView that is passed in.  Then we use the event as the geometry and set the distance for the query.  Then we clone the query so we can use it also for the stats query.  Next we define the array of statDefinitions to count up how many crimes and of what type are found within the query.  Then we do the query against the layerView to get the stats.  Next we use the query with the distance set to highlight the features that are selected.  Here is where we use the highlightHandle from above that we set to null earlier.  We need this because if we don't remove the previous highlighted features they will just accumulate as you drag around the view.
```javascript
function queryStatsOnDrag(layerView, event) {

        // create a query object for the highlight and the statistics query

        var query = layerView.layer.createQuery();
        query.geometry = view.toMap(event); // converts the screen point to a map point
        query.distance = .5; // queries all features within 1 mile of the point
        query.units = "miles";

        var statsQuery = query.clone();

        // Create the statistic definitions for querying stats from the layer view
        // the `onStatisticField` property can reference a field name or a SQL expression
        // `outStatisticFieldName` is the name of the statistic you will reference in the result
        // `statisticType` can be sum, avg, min, max, count, stddev
        var statDefinitions = [

          // total OFFENSES

          {
            onStatisticField: "1",
            outStatisticFieldName: "total",
            statisticType: "count"
          },

          // total types of crime 
          //
          // Since separate fields don't exist for each crime type, we can use
          // an expression to return a 1 or a 0 for each crime type and sum up the
          // results to get the total.

          {
            onStatisticField: "CASE WHEN OFFENSE = 'THEFT/OTHER' THEN 1 ELSE 0 END",
            outStatisticFieldName: "total_Theft",
            statisticType: "sum"
          }, {
            onStatisticField: "CASE WHEN OFFENSE = 'THEFT F/AUTO' THEN 1 ELSE 0 END",
            outStatisticFieldName: "total_TheftAuto",
            statisticType: "sum"
          }, {
            onStatisticField: "CASE WHEN OFFENSE = 'MOTOR VEHICLE THEFT' THEN 1 ELSE 0 END",
            outStatisticFieldName: "total_MVT",
            statisticType: "sum"
          }, {
            onStatisticField: "CASE WHEN OFFENSE = 'ROBBERY' THEN 1 ELSE 0 END",
            outStatisticFieldName: "total_Robbery",
            statisticType: "sum"
          }, {
            onStatisticField: "CASE WHEN OFFENSE = 'ASSAULT W/DANGEROUS WEAPON' THEN 1 ELSE 0 END",
            outStatisticFieldName: "total_AssaultWeapon",
            statisticType: "sum"
          }, {
            onStatisticField: "CASE WHEN OFFENSE = 'BURGLARY' THEN 1 ELSE 0 END",
            outStatisticFieldName: "total_Bulglary",
            statisticType: "sum"
          }, {
            onStatisticField: "CASE WHEN OFFENSE = 'SEX ABUSE' THEN 1 ELSE 0 END",
            outStatisticFieldName: "total_SexAbuse",
            statisticType: "sum"
          }, {
            onStatisticField: "CASE WHEN OFFENSE = 'HOMICIDE' THEN 1 ELSE 0 END",
            outStatisticFieldName: "total_Homicide",
            statisticType: "sum"
          }, {
            onStatisticField: "CASE WHEN OFFENSE = 'ARSON' THEN 1 ELSE 0 END",
            outStatisticFieldName: "total_Arson",
            statisticType: "sum"
          }

        ];

        // add the stat definitions to the the statistics query object cloned earlier
        statsQuery.outStatistics = statDefinitions;

        // execute the query for all features in the layer view
        var allStatsResponse = layerView.queryFeatures(statsQuery)
          .then(function(response) {
            var stats = response.features[0].attributes;
            return stats;
          }, function(e) {
            console.error(e);
          });

        // highlight all features within the query distance
        layerView.queryObjectIds(query)
          .then(function(ids) {
            if (highlightHandle) {
              highlightHandle.remove();
              highlightHandle = null;
            }
            highlightHandle = layerView.highlight(ids);
          });

        // Return the promises that will resolve to each set of statistics
        return promiseUtils.eachAlways([allStatsResponse]);
      }
```
10. The last bit is to write the function to update the numbers with the data returned from the statistic queries.
```javascript
	function updateStats(responses) {
        	var allStats = responses[0].value;
		totalNumber = document.getElementById("tot-crimes");
        	totTheft = document.getElementById("tot-theft");
        	totTheftAuto = document.getElementById("tot-theftAuto");
		totMVT = document.getElementById("tot-MVT");
		totRobbery = document.getElementById("tot-Robbery");
		totAssault = document.getElementById("tot-AssaultwWeapon");
		totBurglary = document.getElementById("tot-Burglary");
		totSexAbuse = document.getElementById("tot-SexAbuse");
		totHomicide = document.getElementById("tot-Homicides");
		totArson = document.getElementById("tot-Arson");
		
        	// Update the total numbers in the title UI element
        	totalNumber.innerHTML = allStats.total;
        	totTheft.innerHTML = allStats.total_Theft;
        	totTheftAuto.innerHTML = allStats.total_TheftAuto;
		totMVT.innerHTML = allStats.total_MVT;
		totRobbery.innerHTML = allStats.total_Robbery;
		totAssault.innerHTML = allStats.total_AssaultWeapon;
		totBurglary.innerHTML = allStats.total_Bulglary;
		totSexAbuse.innerHTML = allStats.total_SexAbuse;
		totHomicide.innerHTML = allStats.total_Homicide;
		totArson.innerHTML = allStats.total_Arson;
      }
```
11. In JSBin, run the app > Select a category to query the feature layer. Click on any point to display the attribute data in a popup.

Your app should look something like this:
* [Code](index.html)
* [Live App](http://jofraley.github.io/Hacking_JavaScript/labs/jsapi/query_stats/index.html)

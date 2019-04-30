###Using the Cedar library to create charts

In this lab you will use the Cedar library to create charts that will dynamically update based on the map extents.  The charts will depict demographic data that includes Total Population, Density, and Median Age for each US State that lies within the map extents.

1.	Click [create_starter_map/index.html](../create_starter_map/index.html) and copy the contents to a new [jsbin.com](http://jsbin.com).

2.	Edit the CSS properties for the #viewDiv and add the CSS properties for the charts:

```css
#viewDiv { height: calc(100% - 300px); width: 100%; }
#chart1, #chart2 { height: 300px; width: 33%; box-shadow: 0px 0px 0px 1px black inset; float: left;}
#chart3 { height: 300px; width: 34%; box-shadow: 0px 0px 0px 1px black inset; float: right;}
```

3.	Next add the JavaScript URLs for the Cedar library. You will need to remove the original link to the ArcGIS JavaScript library and place it below the Cedar library scripts:

```javascript
<script src='https://cdnjs.cloudflare.com/ajax/libs/amcharts/3.21.12/amcharts.js'></script>
<script src='https://cdnjs.cloudflare.com/ajax/libs/amcharts/3.21.12/serial.js'></script>
<script src='https://unpkg.com/@esri/cedar@1.0.0-beta.2/dist/umd/cedar.js'></script>
<script src='https://unpkg.com/@esri/cedar@1.0.0-beta.2/dist/umd/themes/amCharts/calcite.js'></script>
<script src="https://js.arcgis.com/4.11/"></script>
```

4.	Update the require statement and function definition with "esri/geometry/Extent" and "esri/layers/FeatureLayer".  Change the basemap to Topo, edit the map location, and add a feature layer.  This layer depicts the US states and includes multiple demographic attributes.

```javascript
var map = new Map({
  basemap: "topo",
})
      
var extent = new Extent ({
  xmin: -83.7,
  ymin: 36.4,
  xmax: -75.7,
  ymax: 39.6
})
var view = new MapView({
  container: "viewDiv",
  map: map,
  extent: extent
});
		  
var featureLayer = new FeatureLayer("https://services.arcgis.com/P3ePLMYs2RVChkJx/arcgis/rest/services/USA_States_Generalized/FeatureServer/0",
  { outFields: ["*"] }
);

view.map.add(featureLayer);
```

5.	Whenever a user moves around the map we need to update the charts by passing them the visible features.  This is accomplished by watching the view.  After the update completes, a response will be returned which can be used to iterate through each visible graphic.  From each graphic the feature attributes are pushed into an array.  Once the array has been populated by all the visible features it is passed to a function called loadCharts, which it does once for each chart.

```javascript
view.whenLayerView(featureLayer).then(function(layerView) {
  layerView.watch("updating", function(value) {
    if (!value) {
      layerView
        .queryFeatures({
          geometry: view.extent,
          returnGeometry: true
        })
        .then(function(results) {
	  var features = [];
          graphics = results.features;

          graphics.forEach(function(result, index) {
            var attributes = result.attributes;
            var name = attributes.NAME;
	      features.push({
                attributes: {
	          STATE_ABBR: attributes.STATE_ABBR,
		  POPULATION: attributes.POPULATION,
		  POP_SQMI: attributes.POP_SQMI,
		  MED_AGE: attributes.MED_AGE
	        }
	      });
          });
				  
	  loadCharts(features, "chart1", "POPULATION", "Population")
	  loadCharts(features, "chart2", "POP_SQMI", "Density (Square Mile)")
	  loadCharts(features, "chart3", "MED_AGE", "Median Age")
                  
        })
        .catch(function(error) {
           console.error("query failed: ", error);
        });
     }
  });
});
```

6.	The next step is to create loadCharts.  This function generates a bar chart with the cedar library based on the variables that get passed.

```javascript
function loadCharts(features, chartName, chartField, chartLabel){
  features.sort(function(a, b) {
    return b.attributes.POPULATION -a.attributes.POPULATION;
  });
  var definition = {
    type: "bar",
    datasets: [
      {
	data: {
	  features: features
	}
      }
    ],
    series: [
      {
	category: {
	  field: "STATE_ABBR",
	  label: "US State"
      },
	value: {
	  field: chartField,
	  label: chartLabel
	}
      }
    ]
  };
	
  var cedarChart = new cedar.Chart(chartName, definition);
  cedarChart.show();
}
```

7.	The final step is to add the charts to the html below the viewDiv.

```html
<div id="chart1"></div>
<div id="chart2"></div> 
<div id="chart3"></div>
```

 Your app should display a map of the United States.  As you zoom into the map the charts should update dynamically.
 
 It should look something like this:
* [Code](index.html)
* [Live App](http://jofraley.github.io/Hacking_JavaScript/labs/jsapi/cedar_charts/index.html)

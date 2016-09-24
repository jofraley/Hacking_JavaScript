###Feature Layer Search

In this lab you will add a search widget search against a feature layer. The widget performs context-sensitve search as you type and then it will zoom to and highlight the feature selected. You can also format the data in the popup that appears. 

In this lab it will search against the metro stops polygon layer.

1. Click [create_starter_map/index.html](../create_starter_map/index.html) and copy the contents to a new [jsbin.com](http://jsbin.com).

2. In `JSBin` > `HTML`, update the `require` statement and function definition:

    ```javascript
    require(["esri/map",
            // Add Search, FeatureLayer and InfoTemplate modules
            "esri/dijit/Search",
            "esri/layers/FeatureLayer",
            "esri/InfoTemplate",
            "dojo/domReady!"],
      // Add Search alias
      function(Map,Search,FeatureLayer,InfoTemplate) {
        ... 
    ```

3. Add the search widget's div:
 
    ```html
    <body>
      <div id="mapDiv"></div>

      <!-- ADD new div for the search widget -->
      <div id="searchDiv"></div>
    </body>
    ``` 

4. At the top of the page, add the CSS to position `searchDiv`:

    ```CSS
    <style>
      html,body,#mapDiv {
        padding:0;
        margin:0;
        height:100%;
      }

      /* ADD search widget styling */ 
      #searchDiv {
        display: block;
        position: absolute;
        z-index: 2;
        top: 20px;
        left: 74px;
      }
    </style>
    ```

5. Create the Search widget and start it up.

    ```javascript
    function(Map,Search,FeatureLayer,InfoTemplate) {
      map = new Map("mapDiv", {
        center: [-77.029, 38.89],
        zoom: 12,
        basemap: "dark-gray"
      });

      // ADD the search widget 
      var search = new Search({
        map: map
      }, "searchDiv");

      search.startup();
    ```

    At this point, the map will allow you to search against the default ArcGIS Online Geocoding Service. Give it a go. You can enter an address or point of interest (like `Tysons Corner` or `DCA`) or a geography (like `Maryland` or `USA`).

6. Configure the Search Widget to also search against the Metro Stops Feature Service. Insert the following code directly before the `search.startup()` line added above.

    ```javascript
    var sources = search.get("sources");

    sources.push({
      featureLayer: new FeatureLayer("https://services.arcgis.com/lA2FZKuu26Fips7U/ArcGIS/rest/services/MetroStops/FeatureServer/0"),
      name: "Metro Stop Search",
      searchFields: ["NAME"],
      displayField: "NAME",
      exactMatch: false,
      outFields: ["NAME","LINE","ADDRESS"],
      placeholder: "Dupont Circle",
      enableSuggestions: true,

      // Create an InfoTemplate for the popup
      infoTemplate: new InfoTemplate("Metro Stop","Name: ${NAME}</br>Line: ${LINE}</br>Address: ${ADDRESS}")
    });

    search.set("sources",sources);
    ```

7. In JSBin, run the app > select the pulldown on the left-hand side of the search box > select `Metro Stop Search` from the pull down list > enter "Dupont Circle". The app should highlight and zoom into the metro stop point, and a popup should also be displayed.

Your app should look something like this:
* [Code](index.html)
* [Live App](https://jofraley.github.io/Hacking_JavaScript/labs/jsapi3/search_with_widget/index.html)

###Bonus
* Add a [Metro Stops feature layer](https://services.arcgis.com/lA2FZKuu26Fips7U/ArcGIS/rest/services/MetroStops/FeatureServer/0) to the Search widget.
* Customize the popup's CSS. Add the WEB_URL for the stops so they show in the popup.  Hint: see the [Search with customization](https://developers.arcgis.com/javascript/jssamples/search_customized.html) sample.

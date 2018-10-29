###Working with Widgets

In this lab you will add a search widget and search against a feature layer. The widget performs context-sensitve search as you type and then it will zoom to and highlight the feature selected. You can also format the data in the popup that appears. 

In this lab it will search against the neighborhood polygon layer but you can point to any hosted feature layer you want.

1. Click [create_starter_map/index.html](../create_starter_map/index.html) and copy the contents to a new [jsbin.com](http://jsbin.com).

2. In `JSBin` > `HTML`, update the `require` statement and `function` definition:

  ```javascript
  require([
    "esri/WebMap",
    "esri/views/MapView",
    /*** ADD ***/
    "esri/widgets/Search",
    "dojo/domReady!"
  ], function(WebMap, MapView, Search) { 
  ```

3. Create the Search widget and add it to the `top-right` of the UI.

  ```javascript
    ...      

    var view = new MapView({
      container: "viewDiv",
      map: map
    });

    /*** ADD ***/

    // Create search widget
    var searchWidget = new Search({
      view: view
    });

    // Add widget to the UI
    view.ui.add(searchWidget, {
      position: "top-right"
    });    
  ```

  Another advantage of using a web map is that if layers have been configured to search against in the web map they automatically show up in the search widget.  

Your app should look something like this:
* [Code](index.html)
* [Live App](http://jofraley.github.io/Hacking_JavaScript/labs/jsapi/search_with_widget/index.html)

4. Now let's play with some other widgets like a legend widget.  First we need to add the appropiate `require` and `function` definition.

```javascript
  require([
    "esri/WebMap",
    "esri/views/MapView",
    /*** ADD ***/
    "esri/widgets/Search",
    "esri/widgets/Legend",
    "dojo/domReady!"
  ], function(WebMap, MapView, Search, Legend) { 
  ```
5. Now we need to add the widget.

```javascript
  var legend = new Legend({
	    view: view,		
	});
	  
	view.ui.add(legend, "bottom-right");
```
That's it.  play around with moving the legend around on the ui.

6. Next add a basemap gallery to toggle between basemaps.  Use the help this time to figure out what `require` and `function` definition you need as well as how to add the widget to the ui.   
[BasemapGallery Help](https://developers.arcgis.com/javascript/latest/api-reference/esri-widgets-BasemapGallery.html)

Your app should look something like this:
* [Code](index_basemap.html)
* [Live App](http://jofraley.github.io/Hacking_JavaScript/labs/jsapi/search_with_widget/index_basemap.html)

###Bonus
* There is an [Expand Widget](https://developers.arcgis.com/javascript/latest/api-reference/esri-widgets-Expand.html) that acts like an clickable button for opening a widget.  Try putting either or both of the legend and basemap gallery into the expand widget.
```javascript
  var basemapGallery = new Expand ({
	    content: new BasemapGallery({
          view: view,
		  }),
		  view: view,
		  expanded: false
      });
```
Your app should look something like this:
* [Code](index_expand.html)
* [Live App](http://jofraley.github.io/Hacking_JavaScript/labs/jsapi/search_with_widget/index_expand.html)

* Add widgets to your 3D Scene app like the legend and search like what you did in 2D

Your app should look something like this:
* [Code](index_3D_widgets.html)
* [Live App](http://jofraley.github.io/Hacking_JavaScript/labs/jsapi/search_with_widget/index_3D_widgets.html)

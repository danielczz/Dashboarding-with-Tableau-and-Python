# Dashboarding citibike data insights powered by Tableau and Python
Dashboarding repository for citibike analysis in the metropolitan New York City area for the last years.

![Landing page](Resources/Images/anthony-ginsbrook-208979-unsplash.jpg)

## Authors
* Daniel Cespedes - [LinkedIn](https://www.linkedin.com/in/selinzorob/) - [GitHub](https://github.com/danielczz)


## Project Outline

In this project we are going to analyze data generated for the **Earthquakes detected around the world for the last Week**. The data is provided on real time by _USGS - United States Geological Survey_. 

You can review the info on the following link: [USGS GeoJSON Feed](http://earthquake.usgs.gov/earthquakes/feed/v1.0/geojson.php)

We will provide useful actionable insigths about the Earthquakes and his locations around the world in order to have a better visualization and decision making process.


## Technology Landscape

1. JavaScript, one of the core technologies of the World Wide Web.
[_JavaScript_](https://www.javascript.com/)

1. HTML - _Hypertext Markup Language_ is the standard markup language for creating web pages and web applications.
[_HTML_](https://www.w3.org/html/)

1. D3.js - _Data Driven Document for JavaScript_ is a JavaScript library for producing dynamic, interactive data visualizations in web browsers.
[_D3.js_](https://d3js.org/)

1. Leaflet - Leaflet is the leading open-source JavaScript library for mobile-friendly interactive maps.
[_Leaflet_](https://leafletjs.com/)

1. DOM - _The Document Object Model_ is an application programming interface (API) for HTML and XML documents.
[_DOM_](https://www.w3.org/TR/DOM-Level-1/introduction.html)



## Data Analysis Framework

### **Data gathering**
- Data provided for the analysis on JSON representation from USGS:

![Landing page](static/images/JSON.png)



### **Data analysis**

This is a brief sample extraction of the JavaScript code. Find the complete code available here: [_logic.js_](static/js/logic.js)

- Retrieve JSON representation data: 



```JS
// All Week
var queryUrl_EQ = "https://earthquake.usgs.gov/earthquakes/feed/v1.0/summary/all_week.geojson";

// All Month
// var queryUrl_EQ = "https://earthquake.usgs.gov/earthquakes/feed/v1.0/summary/all_month.geojson";


d3.json(queryUrl_EQ, function(data) {
  // Once we get a response, send the data.features object to the createFeatures function
  createFeatures(data.features);
});
```

- Fetch JSON data and create visualizations: 

```JS
function createFeatures(earthquakeData) {

  // Define a function we want to run once for each feature in the features array
  // Give each feature a popup describing the place and time of the earthquake
  function onEachFeature(feature, layer) {

    layer.bindPopup(
      "<h3>" + "Location: " + feature.properties.place +
      "</h3><hr><p>" + "Time: " + new Date(feature.properties.time) +
      "</h3><p>" + "Magnitude: " + feature.properties.mag + "</p>");

  }

  // Create a GeoJSON layer containing the features array on the earthquakeData object
  // Run the onEachFeature function once for each piece of data in the array
  var earthquakes = L.geoJSON(earthquakeData, {
    onEachFeature: onEachFeature,
    pointToLayer: function (feature, layer_circle) {
      var Markers_EQ = {
        radius: 5 * feature.properties.mag,
        fillColor: getColor(feature.properties.mag),
        weight: 1,
        fillOpacity: 0.90
      };
      return L.circleMarker(layer_circle, Markers_EQ)
    }
  });

  console.log(earthquakes);

  // Sending our earthquakes layer to the createMap function
  createMap(earthquakes);
}
```

- Create a portal to geo-isualize Earthquake data by magnitude for last Week: 
```JS
function createMap(earthquakes) {

  // Define streetmap and darkmap layers
  var streetmap = L.tileLayer("https://api.tiles.mapbox.com/v4/{id}/{z}/{x}/{y}.png?access_token={accessToken}", {
    attribution: "Map data &copy; <a href=\"https://www.openstreetmap.org/\">OpenStreetMap</a> contributors, <a href=\"https://creativecommons.org/licenses/by-sa/2.0/\">CC-BY-SA</a>, Imagery © <a href=\"https://www.mapbox.com/\">Mapbox</a>",
    maxZoom: 18,
    id: "mapbox.streets",
    accessToken: API_KEY
  });

  var darkmap = L.tileLayer("https://api.tiles.mapbox.com/v4/{id}/{z}/{x}/{y}.png?access_token={accessToken}", {
    attribution: "Map data &copy; <a href=\"https://www.openstreetmap.org/\">OpenStreetMap</a> contributors, <a href=\"https://creativecommons.org/licenses/by-sa/2.0/\">CC-BY-SA</a>, Imagery © <a href=\"https://www.mapbox.com/\">Mapbox</a>",
    maxZoom: 18,
    id: "mapbox.dark",
    accessToken: API_KEY
  });

  // Define a baseMaps object to hold our base layers
  var baseMaps = {
    "Street Map": streetmap,
    "Dark Map": darkmap
  };

  // Create overlay object to hold our overlay layer
  var overlayMaps = {
    "Earthquakes by magnitude": earthquakes
  };
```

### **Data sharing**
### Screencapture of the final application:

This is a screen-capture and you can find the file here: rief sample extraction of the JavaScript code. Find the complete code available here: [_Screen-capture final application_](static/images/screen.png)

![Screencapture](static/images/screen.png)

# Make a Leaflet Map with Street Tree Data 
## Data provided by the [Urban Resources Institute](https://uri.yale.edu/)

Short URL to this page: [https://tinyurl.com/treemap2018](https://tinyurl.com/treemap2018)

The following tutorial was based on [this awesome tutorial created by Maptime Boston](https://maptimeboston.github.io/leaflet-intro/) as well as tutorials from [the Leaflet website](https://leafletjs.com). We modified it to use our own New Haven street tree data from the neighborhood of Dixwell. We recommend you read through the Maptime Boston tutorial before attempting to try this out. They do a great job explaining all the bits and pieces you need to know!

## Steps

### Part One: Get the data
1) Right-click this [link](https://github.com/EEYale/WebMappingResources) to open this page in a new tab. 
2) Click the green button to `Clone or Download` the data. Then, click `Download Zip`.
3) Unzip the compressed folder and move the `TreesWithLeaflet` folder to your desktop.
4) The important files we will work with are:
	- `index.html` is a working html file that produces a web map that we will see in our browser window. 
	- `shell.html` contains the basic htmp structure we will fill in with code to build our web maps. 
	- `dixwellTrees.geojson` is a file that contains all the tree data that we will be mapping.

### Part Two: Fire up a local web server
1) Open up your command line prompt (Windows) or Terminal (Mac).
2) Type `dir` (Windows) or `ls` (Mac) to get a sense of where you are in your directory. A list of all the folders and files will appear. 
3) Navigate to the directory where you stored the `TreesWithLeaflet` folder using the `cd` command followed by the `directory` name. For example, if my folder is on the desktop. I'll type `cd Desktop` to get to the Desktop, and then `cd TreesWithLeaflet` to enter that folder. For more information on the bash commands you will need to navigate, please visit this [site](https://whatbox.ca/wiki/Bash_Shell_Commands).
4) Now that we are in the directory where all our files and data are, let's fire up a local web server by typing `python -m SimpleHTTPServer` into the command prompt or Terminal. If that doesn't work, try typing in `python -m http.server`. For more detailed instructions on getting a local web server running, go to this [link](https://developer.mozilla.org/en-US/docs/Learn/Common_questions/set_up_a_local_testing_server).
5) Next, open a new browser window. Type `localhost:8000` in the URL bar to see if a map appears. If it does? Celebrate and explore the map. Zoom in and out, click on a point to see what happens. This will be what we hope to build at the end!

### Part Three: Rename your index.html file and restart the web server
1) Now, outside of the command prompt/ Terminal, right-click on the file `index.html` and rename it `finalScript.html`. 
2) Right-click on the file `shell.html` and rename it `index.html`. We will be building our map in the new `index.html` now. 
3) Let's restart the local web server. Go back to the command prompt or Terminal, then press and hold `CTRL` followed by `C`. This shuts down the web server. Then, start it again by pressing the `UP Arrow Key` to get to either the `python -m SimpleHTTPServer` or the `python -m http.server` command. Then, hit `enter` or `return` to start the web server again.
4) Go to your browser. If you refresh the `localhost:8000` browser window, you should see nothing. That's good!

### Part Four: Make a map with just a tile layer.
1) Open `index.html` in your text editor (could be [Sublime](http://www.sublimetext.com) or Notepad or something else). You should see the following: 
```html
<!DOCTYPE html>
<html>
  <head>
    <meta charset="UTF-8">
    <title>title</title>
  </head>
  <body>
  
  </body>
</html>
```
2) To add Leaflet functionality, we need to add both the `Leaflet CSS` as well as the `Leaflet JavaScript library` to our script. Then, we add a `<div>` element which contains our map, and style it by setting the width and height parameters. Finally, we initialize a map using Leaflet and add the [Open Street Map tile layers](https://wiki.openstreetmap.org/wiki/Tiles) as our basemap. Your final index.html file should look like this:
```html
<!DOCTYPE html>
<html>
  <head>
    <meta charset="UTF-8">
    <title>Map with Tile Layer Only</title>
   <!--Adding Leaflet CSS-->
 <link rel="stylesheet" href="https://unpkg.com/leaflet@1.3.1/dist/leaflet.css"
   integrity="sha512-Rksm5RenBEKSKFjgI3a41vrjkw4EVPlJ3+OiI65vTjIdo9brlAacEuKOiQ5OFh7cOI1bkDwLqdLw3Zg0cRJAAQ=="
   crossorigin=""/>

   <style>
   #mapid { 
   	height: 400px;
   	width: 600px; 
   }
   </style>
  </head>

  <body>
	<div id ="mapid"></div> 
	
	<!-- Everything inside the <script> tags is JavaScript.-->
	<!-- Adding Leaflet JavaScript Library. Make sure you put this AFTER Leaflet's CSS -->
 	<script src="https://unpkg.com/leaflet@1.3.1/dist/leaflet.js" integrity="sha512-/Nsx9X4HebavoBvEBuyp3I7od5tA0UzAxs+j83KgC8PU0kgB4XiK4Lfe4y4cgBtaRJQEIFCW+oC506aPT2L1zw==" crossorigin=""></script>

	<!-- Let's make a map! -->
	<script>
	// Initializes the Map, centers the map on New Haven's coordinates, sets zoom level to 14.
	var mymap = L.map('mapid').setView([41.31, -72.93], 14);
	
	// Add Open Street Map Tiles to the map
	L.tileLayer('http://{s}.tile.openstreetmap.org/{z}/{x}/{y}.png', {
		maxZoom: 19,
		attribution: '&copy; <a href="http://www.openstreetmap.org/copyright">OpenStreetMap</a>'
	}).addTo(mymap);
	</script>
```

Your map should look like this:
![alt text](https://raw.githubusercontent.com/EEYale/WebMappingResources/master/TreesWithLeaflet/tilelayeronly.png "Map with Tile Layer")

### Part Five: Make a map with a tile layer and tree data.
Now, let's add some tree data to our map as a set of points. We also want each point to have a popup that tells us the Common Name, Diameter at Breast Height (DBH) and the nearest address for the tree. To do this, we need to add the `jQuery JavaScript library` to pull in data from our `dixwellTrees.geojson` file. Next, after pulling in the data as `geojson` features, we bind each feature to a pop-up and specify the text which should appear. The pop-up pulls from the properties of each `geojson` feature to produce the correct data for each tree/point on the map. Your final index.html file should look like this:
```html
<!DOCTYPE html>
<html>
  <head>
    <meta charset="UTF-8">
    <title>Map with Tile Layer and Tree data</title>
   <!--Adding Leaflet CSS-->
 	<link rel="stylesheet" href="https://unpkg.com/leaflet@1.3.1/dist/leaflet.css"
   integrity="sha512-Rksm5RenBEKSKFjgI3a41vrjkw4EVPlJ3+OiI65vTjIdo9brlAacEuKOiQ5OFh7cOI1bkDwLqdLw3Zg0cRJAAQ=="
   crossorigin=""/>

   <style>
   #mapid { 
   	height: 400px;
   	width: 600px; 
   }
   </style>
  </head>

  <body>
	<div id ="mapid"></div> 
	
	<!-- Everything inside the <script> tags is JavaScript.-->
	<!-- Adding Leaflet JavaScript Library. Make sure you put this AFTER Leaflet's CSS -->
 	<script src="https://unpkg.com/leaflet@1.3.1/dist/leaflet.js" integrity="sha512-/Nsx9X4HebavoBvEBuyp3I7od5tA0UzAxs+j83KgC8PU0kgB4XiK4Lfe4y4cgBtaRJQEIFCW+oC506aPT2L1zw==" crossorigin=""></script>

 	<!-- Add jQuery JavaScript Library-->
 	 <script src="http://code.jquery.com/jquery-2.2.4.js" integrity="sha256-iT6Q9iMJYuQiMWNd9lDyBUStIq/8PuOW33aOqmvFpqI=" crossorigin="anonymous"></script>

	<!-- Let's make a map! -->
	<script>
	// Initializes the Map, centers the map on New Haven's coordinates, sets zoom level to 14.
	var mymap = L.map('mapid').setView([41.31, -72.93], 14);
	
	// Add Open Street Map Tiles to the map
	L.tileLayer('http://{s}.tile.openstreetmap.org/{z}/{x}/{y}.png', {
		maxZoom: 19,
		attribution: '&copy; <a href="http://www.openstreetmap.org/copyright">OpenStreetMap</a>'
	}).addTo(mymap);
	</script>
	
	<script>
	// This code block shows each point with custom popups
	// The following $.getJSON command is enabled by jQuery. 
		// Load GeoJSON from an external file
		$.getJSON("dixwellTrees.geojson",function(data){
		// add GeoJSON layer to the map once the file is loaded
	 	L.geoJSON(data, {
	 			// Bind a popup with custom text to each feature
	 			onEachFeature: function(feature, layer) {
				layer.bindPopup("<strong> Common Name: </strong>" + feature.properties.CommonName + "<br/><strong>DBH: </strong>" + feature.properties.DBH + " inches<br/><strong>Address: </strong>" + feature.properties.Address);
			}
	 	}).addTo(mymap);
 	});
	</script>
  </body>
</html>
```

Your map should look like this:
![alt text](https://raw.githubusercontent.com/EEYale/WebMappingResources/master/TreesWithLeaflet/treedata.png "Map with Tile Layer and Tree Data")

### Part Six: Make a map with a tile layer, tree data and marker cluster capability.
Wow, that was a lot of points. Perhaps we want to show this in a less dizzying way by clustering the points. To do so, we need to add the `Leaflet markerCluster CSS` and `Leaflet markerCluster JavaScript library` to our script. The[markerCluster plugin](https://github.com/Leaflet/Leaflet.markercluster) is just one of the many plugins developed to extend what Leaflet can do. Then, we replace the previous codeblock that binds pop-ups to each feature using the `onEachFeature` method, with some code which uses the `pointToLayer` method instead. Your final index.html file should look like this:
```html
<!DOCTYPE html>
<html>
  <head>
    <meta charset="UTF-8">
    <title>Map with Tile Layer, Tree data and Marker Cluster Functionality</title>
   <!--Adding Leaflet CSS-->
 	<link rel="stylesheet" href="https://unpkg.com/leaflet@1.3.1/dist/leaflet.css"
   integrity="sha512-Rksm5RenBEKSKFjgI3a41vrjkw4EVPlJ3+OiI65vTjIdo9brlAacEuKOiQ5OFh7cOI1bkDwLqdLw3Zg0cRJAAQ=="
   crossorigin=""/>

   <!-- Adding markerCluster default CSS-->
   <link rel="stylesheet" href="https://unpkg.com/leaflet.markercluster@1.3.0/dist/MarkerCluster.Default.css"/> 

   <style>
   #mapid { 
   	height: 400px;
   	width: 600px; 
   }
   </style>
  </head>

  <body>
	<div id ="mapid"></div> 
	
	<!-- Everything inside the <script> tags is JavaScript.-->
	<!-- Adding Leaflet JavaScript Library. Make sure you put this AFTER Leaflet's CSS -->
 	<script src="https://unpkg.com/leaflet@1.3.1/dist/leaflet.js" integrity="sha512-/Nsx9X4HebavoBvEBuyp3I7od5tA0UzAxs+j83KgC8PU0kgB4XiK4Lfe4y4cgBtaRJQEIFCW+oC506aPT2L1zw==" crossorigin=""></script>

 	<!-- Add jQuery JavaScript Library-->
 	 <script src="http://code.jquery.com/jquery-2.2.4.js" integrity="sha256-iT6Q9iMJYuQiMWNd9lDyBUStIq/8PuOW33aOqmvFpqI=" crossorigin="anonymous"></script>

 	 <!-- Adding Marker Cluster JavaScript Library -->
	 <script src="https://unpkg.com/leaflet.markercluster@1.3.0/dist/leaflet.markercluster-src.js"></script> 

	<!-- Let's make a map! -->
	<script>
	// Initializes the Map, centers the map on New Haven's coordinates, sets zoom level to 14.
	var mymap = L.map('mapid').setView([41.31, -72.93], 14);
	
	// Add Open Street Map Tiles to the map
	L.tileLayer('http://{s}.tile.openstreetmap.org/{z}/{x}/{y}.png', {
		maxZoom: 19,
		attribution: '&copy; <a href="http://www.openstreetmap.org/copyright">OpenStreetMap</a>'
	}).addTo(mymap);
	</script>
	
	<script>
	// This code block shows each point with custom popups AND marker cluster functionality
	// The following code is enabled by jQuery. 
	// Load GeoJSON from an external file
	$.getJSON("dixwellTrees.geojson",function(data){
		// add GeoJSON layer to the map once the file is loaded
	 	var trees = L.geoJSON(data, {
	 			pointToLayer: function(feature, latlng){
				var marker = L.marker(latlng);
				marker.bindPopup("<strong> Common Name: </strong>" + feature.properties.CommonName + "<br/><strong>DBH: </strong>" + feature.properties.DBH + " inches<br/><strong>Address: </strong>" + feature.properties.Address);
				return marker;
	 			}
		});
 		
		// Adds a variable called clusters from the markerCluster library
 		var clusters = L.markerClusterGroup();
	
		// Tell it what to cluster by adding the layer of tree points to the clusters. 
 		clusters.addLayer(trees);
	
		// Display this new cluster layer on the map!
 		mymap.addLayer(clusters);
 	});
	</script>
  </body>
</html>
```

Your map should look like this:
![alt text](https://raw.githubusercontent.com/EEYale/WebMappingResources/master/TreesWithLeaflet/markercluster.png "Map with Tile Layer, Tree Data and Marker Cluster Functionality")

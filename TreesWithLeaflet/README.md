# Make a Leaflet Map with Street Tree Data 
## Data provided by the [Urban Resources Institute](https://uri.yale.edu/)

Short URL to this page: [https://tinyurl.com/treemap2018](https://tinyurl.com/treemap2018)

The following tutorial was based on [this awesome tutorial created by Maptime Boston](https://maptimeboston.github.io/leaflet-intro/). We modified it to use our own New Haven street tree data from the neighborhood of Dixwell. We recommend you read through the Maptime Boston tutorial before attempting to try this out. They do a great job explaining all the bits and pieces you need to know!

## Steps

### Part One: Get the data
1) Right-click this [link](https://github.com/EEYale/WebMappingResources) to open this page in a new tab. 
2) Click the green button to `Clone or Download` the data. Then, click `Download Zip`.
3) Unzip the compressed folder and move the `TreesWithLeaflet` folder to your desktop.
4) You will see three files. `index.html` is a working html file that produces a web map that we will see in our browser window. `shell.html` is an empty "skeleton file" that we will use to build the web map. `dixwellTrees.geojson` is a file that contains all the tree data that we will be mapping.

### Part Two: Fire up a local web server
1) Open up your command line prompt (Windows) or Terminal (Mac).
2) Type `dir` (Windows) or `ls` (Mac) to get a sense of where you are in your directory. A list of all the folders and files will appear. 
3) Navigate to the directory where you stored the `TreesWithLeaflet` folder using the `cd` command followed by the `directory` name. For example, if my folder is on the desktop. I'll type `cd Desktop` to get to the Desktop, and then `cd TreesWithLeaflet` to enter that folder. For more information on the bash commands you will need to navigate, please visit this [site](https://whatbox.ca/wiki/Bash_Shell_Commands).
4) Now that we are in the directory where all our files and data are, let's fire up a local web server by typing `python -m SimpleHTTPServer` into the command prompt or Terminal. If that doesn't work, try typing in `python -m http.server`. For more detailed instructions on getting a local web server running, go to this [link](https://developer.mozilla.org/en-US/docs/Learn/Common_questions/set_up_a_local_testing_server).
5) Next, open a new browser window. Type `localhost:8000` in the URL bar to see if a map appears. If it does? Celebrate and explore the map. Zoom in and out, click on a point to see what happens. This will be what we hope to build at the end!

### Part Three: Rename your index.html file and restart the web server
1) Now, outside of the command prompt/ Terminal, right-click on the file `index.html` and rename it `finalScript.html`. 
2) Right-click on the file `shell.html` and rename it `index.html`. We will be building our map in there now. 
3) Let's restart the local web server. Go back to the command prompt or Terminal, then press and hold `CTRL` followed by `C`. This shuts down the web server. Then, start it again by pressing the `UP Arrow Key` to get to either the `python -m SimpleHTTPServer` or the `python -m http.server` command. 
4) Go to your browser. If you refresh the `localhost:8000` browser window, you should see nothing. That's good!

### Part Four: Make a map with a tile layer.
1) Let's build a map. First we are going to add the `Leaflet CSS` and `Leaflet JavaScript library` into our new and relatively empty `index.html` file. 
2) 

### Part Five: Make a map with a tile layer and tree data.

### Part Six: Make a map with a tile layer, tree data and marker cluster capability.




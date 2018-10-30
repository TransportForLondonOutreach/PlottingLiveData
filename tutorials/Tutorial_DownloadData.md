Citymapper is a popular app used by Londoners every day to help them travel from place to place on public transport.
Transport for London provide Citymapper with all the information they need using TfL's publicly available Application Programming Interface (API). 

This excercise will show you how easy it is to get started building a website like Citymapper that shows arrival information on a real map of London, on demand. We'll introduce you to the programming language **JavaScript**, the core language used to write websites and web apps. For this exercise we'll also be using CodePen - a free to use resource that allows you to write real HTML, Javascript and CSS in your browser and instantly see what your code does when you stop typing. CodePen is a great tool to use when learning how to create websites!

# Open your online code editor

The first step is to open [this site](https://stackblitz.com/edit/typescript-v1mxv4?file=GetMap.ts). 

This is your code editor. On the left you type your code, and on the right is the live view that shows you what your code does.

This is what your screen should look like:

![Input](https://raw.githubusercontent.com/TransportForLondonOutreach/PlottingLiveData/master/pics/startscreen.png)

# Adding a map to your page

The first thing we need to do is create a map to plot the station points on. To do this, we need to write our first line of code.

On line 5, below 
````js
export default async functionGetMap(){
````
type:
````js
var map = new BingMap();
````

Then, the background of a map should appear on the right hand side of your screen. 

**Don't forget the semi-colon and the open and close brackets!**

## What does this code do?
`var map` is creating a new `var`iable called `map`. Then this variable is creating a `new BingMap()`. Finally, to finish the creation of the map, we add a `;` to the end of the code - similar to a full stop at the end of a sentence. 


# Adding a marker point to your map

Now that we have the map, we can add a point.

To do this, below 
`var map = new BingMap();`

type this code on line 8:

`map.AddPoint(51.501476, -0.140634, "The Queen's House", "Blue");`

## What does this code do?
`map.AddPoint` is adding a point to the map. In the brackets, we are giving it the **latitude**, **longitude**, **label**, and **colour** of the point.

Again, don't forget the `;` at the end.

## Your code should look like this

<Answer>
```js
import BingMap from "./BingMap"
import Data from "./Data"

export default async function GetMap() {

var map = new BingMap();

  map.AddPoint(51.501476, -0.140634, "The Queen's House", "Blue");

}
```
</Answer>


## What next?
Next we are going to add a pop up label to the Queen's House when you click on it. 


# Adding a pop-up label
If we want to annotate the point with a pop-up label, we need to add the following code to the next lines,:
````js
function queensHousePopUp(){
return "The Queen's House"
}
````
**after this** we need to add `queensHousePopUp` after the `colour` in the `map.AddPoint` brackets. See the comparison box below.

```js
var map = new BingMap();

  map.AddPoint(51.501476, -0.140634, "The Queen's House", "Blue");

  function queensHousePopUp(){
  return "The Queen's House"
  }
```vs
var map = new BingMap();

  map.AddPoint(51.501476, -0.140634, "The Queen's House", "Blue", queensHousePopUp);

  function queensHousePopUp(){
  return "The Queen's House"
  }
```

Your right hand screen should now demonstrate something like this:
![Input](https://raw.githubusercontent.com/TransportForLondonOutreach/PlottingLiveData/master/pics/queenshouse.png)

# Stretch activity
Using [this website](https://www.gps-coordinates.net/), enter your home address in the `address` bar, and click `get GPS coordinates`

Use the Lat, Lon generated to plot your home on the map, replacing "The Queen's House". See if you can change the colour, label, and  to something you like! 

# Plotting information and Live TfL Data
Following on from plotting the Queen's house, we are now going to use Transport for London's live data to plot each station on the Jubilee line, and display the *live* tube times for each station on the line.

## Accessing TfL live data
Transport for London makes live tube data freely accessible to anyone who wants to build an app with the data. 

To view the stop points for the Jubilee line, you need to navigate to this URL:
https://api.tfl.gov.uk/Line/jubilee/StopPoints


As well as this link, you can view LIVE arrival times by navigating to this URL:
https://api.tfl.gov.uk/Line/jubilee/Arrivals/


However, when you navigate to either of these URLs, they display a lot of gobbledegook that isn't human readable. However, it is readable by code that you are about to write next.

## Something to remember

As you can see, the layout for accessing the station location data is:
`https://api.tfl.gov.uk`/`Line`/`ChosenLineHere`/`StopPoints`

and the layour for accessing the station arrival time data is:

`https://api.tfl.gov.uk`/`Line`/`ChosenLineHere`/`Arrivals`

**Remember this if you get to the stretch goal later on!**

# Plotting the Jubilee Line on your map
To add the Jubilee stations to your map, we need to create a new variable called `jubileeLine`, similar to what we did earlier when we created a new variable called `bingMap`. Create a variable on line 13 called `jubileeLine`. If you are having trouble, click reveal to see the answer below.

<Answer>

* var jubileeLine

</Answer>

Now we need to make that variable that will get the location of the stations from TfL. We do this by adding the following on the same line as our new jubileeLine variable:
```js
 = await Data.CallLine("https://api.tfl.gov.uk/Line/jubilee/StopPoints");
```

Your new code should look like the box on the right

```js
var jubilee
```vs
var jubilee = await Data.CallLine("https://api.tfl.gov.uk/Line/jubilee/StopPoints");
```

## What does this code do?
Here, we are making the new jubileeLine variable 'await' for a 'call' to the StopPoints URL.

# Your code should now look like this:
````js
import BingMap from "./BingMap"
import Data from "./Data"

export default async function GetMap() {

  var map = new BingMap();

  map.AddPoint(51.501476, -0.140634, "The Queen's House", "Blue", queensHousePopUp);

  function queensHousePopUp(){
return "The the Queen's House"
}
var jubileeLine = await Data.CallLine("https://api.tfl.gov.uk/Line/jubilee/StopPoints");
````


Next, we are going to display this data, by using a loop. Type the following code starting on line 15. 

````js
for (let station of jubileeLine) {
    
    map.AddPoint(station.lat, station.lon, station.commonName, "Grey")
  
    }
````

## What does this code do?
This `for` loop is telling us that `for` every `station` location of the `jubileeLine` provided by TfL, we want to add a point to our map, using `map.AddPoint`. 

`station.lat`, `station.lon`, `station.commonName` and `"Grey"` in the brackets after `map.AddPoint` are the properties given to make sure the points are plotted in the correct location, with the correct name and the correct color.

Zoom in and have a play with the map!

# Code check
Before we continue, lets just have a quick code check to make sure you're on track!

Your code should now be looking like this:

````js
import BingMap from "./BingMap"
import Data from "./Data"

export default async function GetMap() {

  var map = new BingMap();

  map.AddPoint(51.501476, -0.140634, "The Queen's House", "Blue", queensHousePopUp);

  function queensHousePopUp(){
return "This is the Queen's House"
}

  var jubileeLine = await Data.CallLine("https://api.tfl.gov.uk/Line/jubilee/StopPoints");

  for (let station of jubileeLine) {
    
    map.AddPoint(station.lat, station.lon, station.commonName, "Grey");

  }
  
}
````

If you have any questions or issues, ask one of the team now!

# Adding Live Arrivals
In this section we are now going to introduce live arrivals when you click on the map point. This will make our map act similar to CityMapper!

## Adding a show arrivals property to map.AddPoint
To make sure our station points display the arrivals, we need to add `ShowArrivals` to the map.AddPoint properties on **line 18**. Give it a go... if you're stuck and have no idea, have a look by clicking reveal below.

<Answer>
````js
map.AddPoint(station.lat, station.lon, station.commonName, "Grey");
```vs
map.AddPoint(station.lat, station.lon, station.commonName, "Grey", ShowArrivals);
````
</Answer>

## Create a Show Arrivals function
Next we are going to create a function called ShowArrivals. We do this by typing the following code:
````js
async function ShowArrivals() {
````

## Write the code to make Show Arrivals appear on the map
From line 22 we are going to type the following:
````js
var data = await Data.CallArrivals("https://api.tfl.gov.uk/Line/jubilee/Arrivals/" + station.id)
      var arrivals = Data.List(data);

      var sortedArrivals = Data.Sort(arrivals);

      return Data.Tablify(sortedArrivals);
````

## What does this code do?
We are creating a `var`iable called `data` that `CallArrivals` from TfL from the Jubilee line for each `station.id`.

On the next line we make the `var`iable `arrivals` into 

Next, we make a `var`iable called `sortedArrivals` to arrange the `arrivals` variable from lowest to highest using `Data.Sort`

Finally, we are `return`ing the `sortedArrivals` to the map in a nice looking table format using `Data.Tablify`

If you have any questions about this code, don't hesitate to ask a volunteer!


# Code Check
Congratulations! We've now finished the exercise. Lets check your code to see the finished product!

<Answer>
````js
import BingMap from "./BingMap"
import Data from "./Data"

export default async function GetMap() {

  var map = new BingMap();

  map.AddPoint(51.501476, -0.140634, "The Queen's House", "Blue", queensHousePopUp);

  function queensHousePopUp(){
return "This is the Queen's House"
}

  var jubileeLine = await Data.CallLine("https://api.tfl.gov.uk/Line/jubilee/StopPoints");

  for (let station of jubileeLine) {
    
    map.AddPoint(station.lat, station.lon, station.commonName, "Grey", ShowArrivals);

    async function ShowArrivals() {
      
      var data = await Data.CallArrivals("https://api.tfl.gov.uk/Line/jubilee/Arrivals/" + station.id)
      var arrivals = Data.List(data);

      var sortedArrivals = Data.Sort(arrivals);

      return Data.Tablify(sortedArrivals);
      
    }
  }
}
````

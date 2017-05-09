## Add Real-time Data

I'm pulling in data from this [site](http://www.myshiptracking.com). The site constructs data requests like [this](http://www.myshiptracking.com/requests/vesselsonmap.php?type=json&minlat=10&maxlat=30&minlon=-145&maxlon=-125&zoom=7)

We can construct queries with a latlong bounding box (but it will return null if the bounding box exceeds ~25 degrees each of latitude and longitude). It updates boats as new data comes in... seems to be a TTL in most cases of around 60 seconds, but each request seems to return at least a few new coordinates. The api maps the 'TYPE' key to "10" for fishing vessels.[Here](https://drive.google.com/file/d/0B5u4PuXicC6vRG5UX1lFcjJkZjQ/view?usp=sharing) is a working map and all necessary files with a working api request. Run it on your machine with the python line (python -m simpleHTTPServer 8080
) for now. Alternatively, you can also view the file://… object in Safari, and under “develop" menu, set “Disable Cross Origin Restrictions.”

![sketch](https://s-media-cache-ak0.pinimg.com/564x/55/02/36/550236a9437c439dabdb17ce50d3784b.jpg)

### EXAMPLE ([LINK](http://shangyanyan.me/mpa/))
```
<!DOCTYPE html>
<html>
<head>
	<title>Marine Protected Areas</title>

	<style>
		path.country{
			stroke:#8EC0E4;
			fill:#4ea1d3;
			stroke-width:.5px;
			fill-opacity:.8;
		}

		#sphere {
			fill:#454552;
			stroke:#fff;}

			text{
				font-family: "Menlo"; 
				fill:#eee;

			}

			path.mpa{
				stroke:#e85a71;
				stroke-width:1px;
				fill:#e85a71;
				fill-opacity:.5;
			}

			circle.boat{
				fill:#0ca;
				fill-opacity:.5;
				stroke-width:.5px;
				stroke:white;
			}

		</style>
	</head>

	<body>

		<script src="https://d3js.org/d3.v4.min.js"></script>
		<script src="d3-geo-projection.v1.min.js" ></script>

		<script>

//page variables
var width = 1400;
var height = 1000;

//set up our projection (like a scale, but for two values!)
var projection = d3.geoPeirceQuincuncial()
.scale(200)
.translate([width / 2, height / 2])
//.rotate([0,0,0])
//.center([0,0])
//.clipAngle(180)
;

//set up path generator (like a scale, but for curves!)
var path = d3.geoPath()
.projection(projection);

//setup zoom limits and behavior
var zoom = d3.zoom()
.scaleExtent([1, 12])
.on("zoom", zoomed);

//create svg container
var svg = d3.select("body")
.append("svg")
.attr("width", width)
.attr("height", height)
.style("background-color","#454552")
;

//attach zoom listener to the svg object
svg
.call(zoom); 

//add svg group to hold geography
var g = d3.select("svg").append("g");

//draw oceans
g.append("path")
.datum({type: "Sphere"})
.attr("class", "sphere")
.attr("id","sphere")
.attr("d", path)
;

//draw country maps
d3.json("world-110m.json", function(err, geojson) {
	g.selectAll(path.country)
	.data(geojson.features)
	.enter()
	.append("path")
	.attr("d", path)
	.classed("country",true)
	;

//draw marine protected areas and define tooltips
d3.json("mpa7500_02.json", function(err, geojson) {
	g.selectAll(path.mpa)
	.data(geojson.features)
	.enter()
	.append("path")
	.attr("d", path)
	.classed("mpa",true)
	.on("mouseover", function(d){
		if (d.properties.designation_eng == ""){
			var engdes = "Protected Area";
		}
		else{
			var engdes = d.properties.designation_eng;
		}

		if (d.properties.no_take_area == null){
			var notake = "Status Unknown";
		}
		else{
			var notake = d.properties.no_take_area + "km^2";
		}

		tooltip.text(
			d.properties.name + " | " + engdes + " | " + "No-Take Area: " + notake
			)
		.attr("y", (d3.event.pageY)+"px")
		.attr("x",(d3.event.pageX)+"px")
		.transition()
		.duration(500)
		.attr("fill-opacity", "1")
		;

	})
	.on("mouseout", function(d){
		tooltip
		.transition()
		.duration(500)
		.attr("fill-opacity", "0")
	})

	;

//make tooltip container
var tooltip = d3.select("svg")
.append("text")
.attr("fill-opacity", "0")
.text("tooltip!")
.classed("tooltip", true);

//region settings
var minlat = 10;
var maxlat = 30;
var minlon = -145;
var maxlon = -120;

//Undocumented API Notes
//d.MMSI = Unique Identifier with Embedded Country (https://help.marinetraffic.com/hc/en-us/articles/205220087-Which-way-is-information-on-a-vessel-s-flag-found-)
//d.NAME = Vessel Name
//d.SOG = Speed in Knots
//d.COG = Heading in Clockwise Degrees (0 = North; 90 = West...)
//d.DEST = Destination Port
//d.TYPE = Vessel Type (7=Vehicle Carrier, 8:Tanker...)
//d.LAT = Current Latitude
//d.LNG = Current Longitude
//d.ARV = Projected Arrival Time, 201704272230 is 10:30PM on April 27, 2017 GMT

//draw boats
d3.json("http://www.myshiptracking.com/requests/vesselsonmap.php?type=json&minlat=" + minlat + "&maxlat=" + maxlat + "&minlon=" + minlon + "&maxlon=" + maxlon + "&zoom=7", function(err, boats) {
	console.log(boats)
	g.selectAll("circle.boat")
	.data(boats[0].DATA)
	.enter()
	.append("circle")
	.attr("transform", function(d) {return "translate(" + projection([d.LNG, d.LAT]) + ")";})
	.attr("r",3)
	.classed("boat",true)
	.on("mouseover", function(d){

		tooltipBoat.text(
			"Vessel: " + d.NAME + ' | Heading: ' + d.DEST
			)
		.attr("y", (d3.event.pageY)+"px")
		.attr("x",(d3.event.pageX)+"px")
		.transition()
		.duration(500)
		.attr("fill-opacity", "1")
		;

	})
	.on("mouseout", function(d){
		tooltipBoat
		.transition()
		.duration(500)
		.attr("fill-opacity", "0")
	})

});

//make tooltip container
var tooltipBoat = d3.select("svg")
.append("text")
.attr("fill-opacity", "0")
.text("tooltip!")
.classed("tooltipBoat", true);

})

})

//what happens when a zoom is recognized?
function zoomed() {
g.attr("transform", d3.event.transform); // updated for d3 v4
}

</script>

</body>
</html>
```

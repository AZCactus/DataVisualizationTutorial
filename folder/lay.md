## Overlay No-take Zone

[Zach Pino](http://www.zachpino.com/) managed to hack together a script that combed through the gdb file we found and convert it to geojson. The data and coordinates were sloppy and incomplete, so he combined it with some other resources (namely from CIA factbook and NaturalEarthData) to produce a composite geojson file. All property data was retained, but you'll see a lot of "unknown"s and "null"s.

Also, a note about projection choice. Since I am presenting primarily oceanic information, I am exploring d3's [built-in options](https://github.com/d3/d3-geo#azimuthal-projections) as well as [alternate projections](https://github.com/d3/d3-geo-projection). Most common projections are optimized for land areas, and are misleading at best when used to project oceanic datasets. Definitely be looking especially at projections termed "equal area" in making my choice. Equidistant projections are going to distort water areas badly... You'll see I'm using an unusual projection in this map called Peirce's Quincuncial, which nonuniformly pulls the earth's sphere into a square. It makes the oceans easier to visualize, at the cost of continental accuracy around the equator. It is not equal area, but it only distorts 3.2% areally at the equator.

![sketch](https://s-media-cache-ak0.pinimg.com/originals/12/43/1d/12431de73e0ecb98930773fe6633abe0.png)

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

Then, we will learn how to [Add Real-time Data](real.md).



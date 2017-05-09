## Zoom, Label, Interaction

### EXAMPLE
```
<!DOCTYPE html>
<html>
<head>
	<title>Zoomable Map</title>

	<style>
		path.country{
			fill: #4ea1d3;
			stroke-width:.5px;
			stroke: #fff;
		}	

		path.disputed{
			fill:#e85a71;
		}

		text.label{
			font-family:menlo;
			font-size:8px;
			fill:#eee;
		}

		text.tooltip {
			fill:#eee;
			font-family: menlo;
			font-size: 10px;
		}
	</style>
</head>
<body>

	<!-- bring d3 into our file -->
	<script src="https://d3js.org/d3.v4.min.js"></script>

	<script>

//variables for page layout	
var width = 1000;
var height = 500;

//defines how our map appears
var projection = d3.geoEquirectangular()
.scale(150)
.translate([width/2,height/2])
//.rotate([lampbda,phi,theta])
//.center([lon,lat])
//.clipAngle(180)
;

//long lat to svg coordinates using the projection defined above.
var path = d3.geoPath()
.projection(projection)
;

var zoom = d3.zoom()
.scaleExtent([.5,12])
.on("zoom", zoomed);

var svg = d3.select("body")
.append("svg")
.attr("width", width)
.attr("height", height)
.style("background-color","#454552")
;

svg.call(zoom);

var g = d3.select("svg").append("g");

d3.json("world-110m.json", function(err,geojson){
	g.selectAll("path.country")
	.data(geojson.features)
	.enter()
	.append("path")
	.attr("d", path)
	.classed("country",true)
	;


/*	
d3.json("world-110m.json", function(err,geojson){
g.selectAll("text.label")
.data(geojson.features)
.enter()
.append("text")
.attr("x",function(d){return path.centroid(d)[0]})
.attr("y",function(d){return path.centroid(d)[1]})
.text(function(d){return d.properties.name})
.attr("text-anchor","middle")
.append("text")
.classed("label",true)
;
});

*/

d3.json("disputed.json", function(err,geojson){
	g.selectAll("path.disputed")
	.data(geojson.features)
	.enter()
	.append("path")
	.attr("d", path)
	.classed("disputed",true)
	.on("mouseover", function(d){
		tooltip
		.text(d.properties.BRK_NAME + "|" + d.properties.NOTE_BRK)
		.attr("x",(d3.event.pageX) + "px")
		.attr("y",(d3.event.pageY) + "px")
		.transition()
		.duration(300)


		.attr("fill-opacity","1")
	})
	.on("mouseout",function(d){
		tooltip
		.transition()
		.duration(300)
		.attr("fill-opacity","0")
	})
	;

	var tooltip = d3.select("svg")
	.append("text")
	.attr("fill-opacity","0")
	.text("tooltip!")
	.classed("tooltip",true)
	;



});
});

function zoomed() {
	g.attr("transform", d3.event.transform);
}

</script>

</body>
</html>
```
With those basic knowledge, we can learn how to [Overlay No-take Zone](lay.md).

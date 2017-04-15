## Transitions

Motion is important for real-time data visualization. So we should learn how to make [Transitions](https://github.com/zachpino/realtimespace/tree/master/week8) of data points to show change of real-time data, which relates to **Dynamic Nature of Spatial Dimension** in section [Sketch Ideas](sketch.md).

### EXAMPLE
```
<!DOCTYPE html>
<html>
<head>
	<title>Transitions</title>
</head>

<style>
</style>

<body>
	<button onclick="drawDots()">Click Me!</button>


	<script src="https://d3js.org/d3.v4.min.js"></script>

	<script>

//python -m SimpleHTTPServer 8080

var width = 800;
var height = 800;

var svg = d3.select("body")
.append("svg")
.attr("width", width)
.attr("height", height)
.style("background-color","#ccc")
;

//on page load
d3.json("mock.json", function(error, data) {

	svg.selectAll(".dots")
	.data(data)
	.enter()
	.append("circle")
	.attr("cx", function(d){return d.x})
	.attr("cy", function(d){return d.y})
	.attr("r", function(d){return d.size})
	.attr("fill", function(d){return d.color})
	.classed("dots", true)
	//same as .attr("class", "dots")
	;

});

function drawDots(){
	//d3.selectAll(".dots")
	//.transition()
	//.duration(1000)
	//.ease(d3.easeQuadIn)
	//.attr("cy",height)

    d3.json("mock.json", function(error, data) {
	
	//save selection in variable name
	var dots = svg.selectAll(".dots")
	.data(data);

	//update
	dots
	.transition()
	.duration(500)
	.attr("r", function(d){return d.size*2})
	.transition()
	.duration(1500)
	.attr("r", function(d){return d.size})
	.attr("cx", function(d){return d.x})
	.attr("cy", function(d){return d.y})
    ;
	
	//enter
	dots
	.enter()
	.append("circle")
	.attr("cx", function(d){return d.x})
	.attr("cy", function(d){return d.y})
	.attr("r", 0)
	.attr("fill", function(d){return d.color})
	.classed("dots", true)
	.transition()
	.duration(1000)
	.attr("r", function(d){return d.size})
	;

	dots
	.exit()
	.transition()
	.duration(3000)
	.ease(d3.easeBounceOut)
	.attr("cy", height)
	.style("fill-opacity",0)
	.remove()
	;

});
	
}

var interval = setInterval(
    function(){
          drawDots();
    }, 5000
    );


</script>


</body>
</html>
```
Then, we will learn [Zoom, Label, Interaction](other.md).

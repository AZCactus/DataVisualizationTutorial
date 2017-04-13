## Code Visualization

**Basics of D3**

Learn the [Basics of D3](https://github.com/zachpino/realtimespace/tree/master/week5). We will learn how to draw some dots with a static dataset to make a simple data visualization.

### MY CODE ([MY DOTS](http://shangyanyan.me/scatter/))
```
<html>
<head>

<body>

<!-- load the d3.js library -->     
<script src="https://d3js.org/d3.v4.min.js"></script>

<script>

//define our block size variables
var width = 1000;
var height =  600;

// set the ranges
var x = d3.scaleLinear().domain([-180,180]).range([0, width]);
var y = d3.scaleLinear().domain([-90,90]).range([height, 0]);


// append the svg obgect to the body of the page
var svg = d3.select("body").append("svg")
    .attr("width", width)
    .attr("height", height)
    .style("background-color","#cde")
;

// Get the data
d3.csv("data.csv", function(error, data) {
  if (error) throw error;

  // format the data
  data.forEach(function(d) {
      d.lat = +d["Latitude"];
      d.long = +d["Longitude"];
      d.pop = +d["2015Pop"];
      d.city = d["City"];
      d.country = d["Country"];
  });

  // Add the scatterplot
  svg.selectAll("dot")
      .data(data)
      .enter()
      .append("circle")
      .attr("r", function(d) { return d.pop / 1000; })
      .attr("cx", function(d) { return x(d.long); })
      .attr("cy", function(d) { return y(d.lat); })
      .attr("stroke","cyan")
      .attr("stroke-width","1")
      .append("title").text(function(d) { return d.city + ", " + d.country; })
;

});

</script>
</body>
```

**Updating Map**

After learning how to pull API, draw map, and use D3, it's time for us to do an exercise to make an [Updating Map](https://github.com/zachpino/realtimespace/tree/master/week6) to combine all the skills togather. 

### MY CODE ([MY MAP](http://shangyanyan.me/REALTIMEMAP/))
```
	        <!DOCTYPE html>
		<html>
		<head>
			<title>Realtime Map</title>
			<style>
				.label{font-family:'menlo';fill: #fff;}
				.country {fill:#acb; stroke: #fff; stroke-width:.5px;}
				#sphere{fill:#268; stroke: #fff; stroke-width:3px;}
				.grat {fill:none; stroke: #fff; stroke-width:.5px;}
			</style>
		</head>
		<body>
			<script src="https://d3js.org/d3.v4.js"></script>

			<script>
				var width = 1200;
				var height = 600;

				var apikey = "5578ca60f4a6a8779dc3ef883dacda6b";
				var cities = "4887442,2037013,1814087,1795269,1850147,1668341,1835848,745044";

		//draw svg container
		var svg = d3.select("body")
		.append("svg").attr("width",width)
		.attr("height",height)
		.style("background-color","#cde")
		;

		//scale area (remapping functions)
		var x = d3.scaleLinear().domain([-180,180]).range([0,width]);
		var y = d3.scaleLinear().domain([-90,90]).range([height,0]);
		var tmp = d3.scaleLinear().domain([-20,40]).range([0,255]);

        //This is my map projection, which is kind of like a scale but for 2 numbers!
        var projection = d3.geoMercator()
        .scale(100)
        .rotate(0,0,0)
        .translate([width/2,height/2]);

        //make path gernerator
        var path = d3.geoPath().projection(projection);

        svg.append("path")
        .datum({type: "Sphere"})
        .attr("id", "sphere")
        .attr("d", path)
        ;
        
        d3.json("world-110m.json", function(error, geojson){
        	svg.append("path")
        	.attr("d", path(geojson))
        	.classed("country", true);

        	var graticule = d3.geoGraticule().step([10,10]);
        	svg.append("path")
        	.datum(graticule)
        	.attr("d", path)
        	.classed("grat", true)
        	;
        });

        function drawMap(){
        	d3.json('http://api.openweathermap.org/data/2.5/group?id=' + cities + "&appid=" + apikey, function(error, weather){

		    //check for error and continue if so
		    if(error) throw error;
		    console.log(weather);

		    svg.selectAll(".dot").remove();

		    svg.selectAll(".dot")
		    .data(weather.list).enter()
		    .append("circle")
		    .attr("transform", function(d){return "translate(" + projection([d.coord.lon,d.coord.lat]) + ")";})
		    .attr("r",function(d){return d.wind.speed;})
		    .attr("fill",function(d){
		    	var tempcolor = d3.rgb(tmp(d.main.temp - 273),0,255 - (tmp(d.main.temp - 273)));
		    	return tempcolor;
		    })
		    .attr("stroke","#fff")
		    .attr("stroke-width",2)
		    .classed("dot",true)
		    ;

		    svg.selectAll(".label").remove();
		    
		    svg.selectAll(".label")
		    .data(weather.list)
		    .enter()
		    .append("text")
		    .text(function(d){return d.name + " " + +(d.main.temp - 273).toFixed(2); })
		    .attr("transform", function(d){return "translate(" + projection([d.coord.lon,d.coord.lat]) + ")";})
		    .attr("font-size" , 10)
		    .classed("label",true)
		    ;



		});
        }

        drawMap();

        var interval = setInterval( function(){
        	drawMap();
        }, 5*1000
        );

    </script>
</body>
</html>
```

**Transitions**

Motion is important for real-time data visualization. So we should learn how to make [Transitions](https://github.com/zachpino/realtimespace/tree/master/week8) of data points to show change of real-time data, which relates to **Dynamic Nature of Spatial Dimension** in section [Sketch Ideas](sketch.md).

**Useful Tools**

[mockaroo](https://www.mockaroo.com/): allows you to quickly and easily to download large amounts of randomly generated test data based on your own specs which you can then load directly into your test environment using SQL or CSV formats.

[JSON Editor Online](http://www.jsoneditoronline.org/): Organizes data structure and hierarchy of JSON file.

**D3 Examples**

We don't always need to make our own codes totally from scratch, because there are already lots of exsiting online resources which we can use. So learning how to modify and combine existing codes can be an essential skill.

Animation

- https://bl.ocks.org/mbostock/3808234
- https://bl.ocks.org/CoreyBurkhart/d42bfa0466ad6a7eb9e0095737a11952
- https://bl.ocks.org/sarubenfeld/372462f3ea07b8ec8ba68d981230a01e
- https://bl.ocks.org/mbostock/346f4d967650b27c0511
- http://blockbuilder.org/search#d3version=v4

Transition

- https://github.com/d3/d3-transition

Geography

- https://bl.ocks.org/mbostock/29cddc0006f8b98eff12e60dd08f59a7
- https://bl.ocks.org/dbetebenner/dc95803c61970d4282e620b83ff2964a
- https://bl.ocks.org/dbetebenner/a9748d40ac9d5dcf2f65809d921819be
- http://blockbuilder.org/search#d3version=v4;d3modules=d3-geo
- https://github.com/d3/d3-geo

[Mike Bostock's Blocks](https://bl.ocks.org/mbostock)

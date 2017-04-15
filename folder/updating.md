## Updating Map

After learning how to pull API, draw map, and use D3, it's time for us to do an exercise to make an [Updating Map](https://github.com/zachpino/realtimespace/tree/master/week6) to combine all the skills togather. 

### EXAMPLE ([LINK](http://shangyanyan.me/REALTIMEMAP/))
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

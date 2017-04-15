## Basics of D3

Learn the [Basics of D3](https://github.com/zachpino/realtimespace/tree/master/week5). We will learn how to draw some dots with a static dataset to make a simple data visualization.

### EXAMPLE ([LINK](http://shangyanyan.me/scatter/))
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

**Useful Tools**

[mockaroo](https://www.mockaroo.com/): allows you to quickly and easily to download large amounts of randomly generated test data based on your own specs which you can then load directly into your test environment using SQL or CSV formats.

[JSON Editor Online](http://www.jsoneditoronline.org/)/[Online JSON Viewer](http://jsonviewer.stack.hu/): Organizes data structure and hierarchy of JSON file.

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

## Finish the Map

The first few digits of the mmsi number tell you which country the boat is from ([example 1](http://s73417h.github.io/MIDs/)&[example 2](http://www.vtexplorer.com/vessel-tracking-mmsi-mid-codes.html), so we can use those digits to assign vessels to the countries they're from. And the final code looks like this: 

```
<html>
<head>
<title>Realtime Map</title>

<link href="reset.css" rel="stylesheet">

<style>

.country:hover{fill:#88B9D3; stroke: #1C3148; stroke-width:.2px;}
.country{fill:#2A4D65; stroke: #1C3148; stroke-width:.2px;}
.grat {fill:none; stroke: #fff; stroke-width:.1px;}
.label{fill:lightgrey; font-size:10px; opacity:0;}
.label:hover{fill:lightgrey; font-size:10px; opacity:1;} 
#sphere {fill:#1C3148; stroke:#fff; stroke-width:.2px;}

#cSelected{fill:#88B9D3;}

path.mpa{stroke:#e85a71; stroke-width:0.1px; fill:#e85a71; fill-opacity:.5;}

a {color:#808080;}
a:hover {color: black;} 

text.label{font-family:menlo; font-size:8px; fill:#eee;}

text.tooltip {fill:#eee; font-family: menlo; font-size: 10px;}

text.tooltipBoat{fill:#eee; font-family: menlo; font-size: 10px;}

html { box-sizing: border-box;}

*, *:before, *:after {box-sizing: inherit;}

#container{width:100%; margin-left: auto; margin-right: auto;}

#containermain{width:100%; float: left;}

.row::after {content: ""; clear: both; display: table;}

.col{float:left; min-height:50px; margin-left:.5%; margin-right:.5%; margin-top:.5%; margin-bottom:.5%;}

.centered{margin-left:auto; margin-right:auto; float:none;}

.one{ width:7.333%; }
.two{ width:15.666%; }
.three{ width:24%; }
.four{ width:32.333%; }
.five{ width:40.666%; }
.six{ width:49%;}
.seven{ width:57.333%; }
.eight{ width:65.666%; }
.nine{ width:74%; }
.ten{ width:82.333%; }
.eleven{ width:90.666%; }
.twelve{ width:99%; }

</style>

</head>

<body>
<div id="container"> 
<div class="row" style="width: 100%; padding-left: 20px;
position: fixed; background-color: white; opacity: 0.98; top: 0px; height: 68px; display: block;">

<div class="col five" style="height: 20px"><h1 style="font-family:Bodoni; font-size: 30px; padding-top: 13px">PROTECT OUR <a href="index.html", style="text-decoration: none;">OCEAN</a></h1></div>

<div class="col two" style="height: 20px"><a href="https://github.com/shangyanyan/DataVisualizationTutorial/blob/master/folder/intent.md", style="text-decoration: none;"><h2 style="font-family:BGaramond; font-size: 15px;text-align: right; padding-top: 20px; padding-right: 55px; text-decoration: none">ABOUT</h2></a></div>

<div class="col one" style="height: 20px"><a href="prismic/index.html", style="text-decoration: none;"><h2 style="font-family:BGaramond; font-size: 15px;text-align: left; padding-top: 20px; text-decoration: none">BLOG</h2></a></div>

<div class="col one" style="height: 20px"><a href="https://github.com/shangyanyan/DataVisualizationTutorial", style="text-decoration: none;"><h2 style="font-family:BGaramond; font-size: 15px;text-align: left; padding-top: 20px; text-decoration: none">TUTORIAL</h2></a></div>

<div class="col three" ><a href="http://shangyanyan.wixsite.com/graduateportfolio", style="text-decoration: none;"><h2 style="font-family:BGaramond; font-size: 15px;text-align: left; padding-top: 20px; padding-left: 30px; text-decoration: none">MORE ABOUT YANYAN</h2></a></div>
</div>

<div class="row" id="content-map" style="padding-top: 70px;">

<!-- bring d3 into our file -->
<script src="https://d3js.org/d3.v4.min.js"></script>
<script src="d3-geo-projection.v1.min.js" ></script>

<script>

//variables for page layout 
var width = 1225;
var height = 1225;

//set up our projection (like a scale, but for two values!)
var projection = d3.geoPeirceQuincuncial()
.scale(275)
.translate([width / 2, height / 2])
;

//long lat to svg coordinates using the projection defined above.
var path = d3.geoPath()
.projection(projection)
;

var zoom = d3.zoom()
.scaleExtent([1,12])
.on("zoom", zoomed);

var svg = d3.select("#content-map")
.append("svg")
.attr("width", width)
.attr("height", height)
.style("background-color","#1C3148")
;

svg.call(zoom);

var g = d3.select("svg").append("g");

//draw oceans
g.append("path")
.datum({type: "Sphere"})
.attr("class", "sphere")
.attr("id","sphere")
.attr("d", path)

d3.json("world-110m.json", function(err,geojson){
g.selectAll("path.country")
.data(geojson.features)
.enter()
.append("path")
.attr("d", path)
.classed("country",true)
.on("mouseover", function(d){
tooltip
.text(d.properties.name)//inside dispute.json, we are looking for BRK_NAME  
.attr("x", (d3.event.pageX-40)+"px")
.attr("y", (d3.event.pageY-40)+"px")
.transition()//animate
.duration(300)
.attr("fill-opacity", "1")
})
.on("mouseout", function(d){
tooltip
.transition()
.duration(300)
.attr("fill-opacity","0")
})
.on("click", function(d){
var clickedCountry = d3.select(this).attr("class")

g.selectAll("#cSelected").attr("id","unSelected");

this.setAttribute('id', 'cSelected');

if (d.properties.name == "USA"){

g.selectAll("circle.USA")
.attr("opacity",0.75)
.transition()
.duration(500)
.attr("r", 4)
.transition()
.duration(500)
.attr("r", 2)}
else{g.selectAll("circle.USA").attr("opacity",.05)}

if (d.properties.name == "Canada"){

g.selectAll("circle.Canada").attr("opacity",0.75)
.transition()
.duration(500)
.attr("r", 4)
.transition()
.duration(500)
.attr("r", 2)}
else{g.selectAll("circle.Canada").attr("opacity",.05)}

if (d.properties.name == "Mexico"){

g.selectAll("circle.Mexico").attr("opacity",0.75)        
.transition()
.duration(500)
.attr("r", 4)
.transition()
.duration(500)
.attr("r", 2)}
else{g.selectAll("circle.Mexico").attr("opacity",.05)}

if (d.properties.name == "China"){

g.selectAll("circle.China").attr("opacity",0.75)        
.transition()
.duration(500)
.attr("r", 4)
.transition()
.duration(500)
.attr("r", 2)}
else{g.selectAll("circle.China").attr("opacity",.05)}

if (d.properties.name == "India"){

g.selectAll("circle.India").attr("opacity",0.75)        
.transition()
.duration(500)
.attr("r", 4)
.transition()
.duration(500)
.attr("r", 2)}
else{g.selectAll("circle.India").attr("opacity",.05)}

if (d.properties.name == "Japan"){

g.selectAll("circle.Japan").attr("opacity",0.75)        
.transition()
.duration(500)
.attr("r", 4)
.transition()
.duration(500)
.attr("r", 2)}
else{g.selectAll("circle.Japan").attr("opacity",.05)}

if (d.properties.name == "South Korea"){

g.selectAll("circle.Korea").attr("opacity",0.75)        
.transition()
.duration(500)
.attr("r", 4)
.transition()
.duration(500)
.attr("r", 2)}
else{g.selectAll("circle.Korea").attr("opacity",.05)}

if (d.properties.name == "Taiwan"){

g.selectAll("circle.Taiwan").attr("opacity",0.75)        
.transition()
.duration(500)
.attr("r", 4)
.transition()
.duration(500)
.attr("r", 2)}
else{g.selectAll("circle.Taiwan").attr("opacity",.05)}

if (d.properties.name == "Australia"){

g.selectAll("circle.Australia").attr("opacity",0.75)        
.transition()
.duration(500)
.attr("r", 4)
.transition()
.duration(500)
.attr("r", 2)}
else{g.selectAll("circle.Australia").attr("opacity",.05)}

if (d.properties.name == "England"){

g.selectAll("circle.England").attr("opacity",0.75)        
.transition()
.duration(500)
.attr("r", 4)
.transition()
.duration(500)
.attr("r", 2)}
else{g.selectAll("circle.England").attr("opacity",.05)}

if (d.properties.name == "Brazil"){

g.selectAll("circle.Brazil").attr("opacity",0.75)        
.transition()
.duration(500)
.attr("r", 4)
.transition()
.duration(500)
.attr("r", 2)}
else{g.selectAll("circle.Brazil").attr("opacity",.05)}

if (d.properties.name == "Russia"){

g.selectAll("circle.Russia").attr("opacity",0.75)        
.transition()
.duration(500)
.attr("r", 4)
.transition()
.duration(500)
.attr("r", 2)}
else{g.selectAll("circle.Russia").attr("opacity",.05)}

if (d.properties.name == "Germany"){

g.selectAll("circle.Germany").attr("opacity",0.75)        
.transition()
.duration(500)
.attr("r", 4)
.transition()
.duration(500)
.attr("r", 2)}
else{g.selectAll("circle.Germany").attr("opacity",.05)}

if (d.properties.name == "France"){

g.selectAll("circle.France").attr("opacity",0.75)        
.transition()
.duration(500)
.attr("r", 4)
.transition()
.duration(500)
.attr("r", 2)}
else{g.selectAll("circle.France").attr("opacity",.05)}

if (d.properties.name == "Spain"){

g.selectAll("circle.Spain").attr("opacity",0.75)        
.transition()
.duration(500)
.attr("r", 4)
.transition()
.duration(500)
.attr("r", 2)}
else{g.selectAll("circle.Spain").attr("opacity",.05)}

if (d.properties.name == "Denmark"){

g.selectAll("circle.Denmark").attr("opacity",0.75)        
.transition()
.duration(500)
.attr("r", 4)
.transition()
.duration(500)
.attr("r", 2)}
else{g.selectAll("circle.Denmark").attr("opacity",.05)}

if (d.properties.name == "Greece"){

g.selectAll("circle.Greece").attr("opacity",0.75)        
.transition()
.duration(500)
.attr("r", 4)
.transition()
.duration(500)
.attr("r", 2)}
else{g.selectAll("circle.Greece").attr("opacity",.05)}

if (d.properties.name == "Netherlands"){

g.selectAll("circle.Netherlands").attr("opacity",0.75)        
.transition()
.duration(500)
.attr("r", 4)
.transition()
.duration(500)
.attr("r", 2)}
else{g.selectAll("circle.Netherlands").attr("opacity",.05)}

if (d.properties.name == "Norway"){

g.selectAll("circle.Norway").attr("opacity",0.75)        
.transition()
.duration(500)
.attr("r", 4)
.transition()
.duration(500)
.attr("r", 2)}
else{g.selectAll("circle.Norway").attr("opacity",.05)}

if (d.properties.name == "Sweden"){

g.selectAll("circle.Sweden").attr("opacity",0.75)        
.transition()
.duration(500)
.attr("r", 4)
.transition()
.duration(500)
.attr("r", 2)}
else{g.selectAll("circle.Sweden").attr("opacity",.05)}

if (d.properties.name == "Vietnam"){

g.selectAll("circle.Vietnam").attr("opacity",0.75)        
.transition()
.duration(500)
.attr("r", 4)
.transition()
.duration(500)
.attr("r", 2)}
else{g.selectAll("circle.Vietnam").attr("opacity",.05)}

if (d.properties.name == "Panama"){

g.selectAll("circle.Panama").attr("opacity",0.75)        
.transition()
.duration(500)
.attr("r", 4)
.transition()
.duration(500)
.attr("r", 2)}
else{g.selectAll("circle.Panama").attr("opacity",.05)}

if (d.properties.name == "South Africa"){

g.selectAll("circle.Africa").attr("opacity",0.75)        
.transition()
.duration(500)
.attr("r", 4)
.transition()
.duration(500)
.attr("r", 2)}
else{g.selectAll("circle.Africa").attr("opacity",.05)}

if (d.properties.name == "Peru"){

g.selectAll("circle.Peru").attr("opacity",0.75)        
.transition()
.duration(500)
.attr("r", 4)
.transition()
.duration(500)
.attr("r", 2)}
else{g.selectAll("circle.Peru").attr("opacity",.05)}

if (d.properties.name == "Indonesia"){

g.selectAll("circle.Indonesia").attr("opacity",0.75)        
.transition()
.duration(500)
.attr("r", 4)
.transition()
.duration(500)
.attr("r", 2)}
else{g.selectAll("circle.Indonesia").attr("opacity",.05)}

if (d.properties.name == "Chile"){

g.selectAll("circle.Chile").attr("opacity",0.75)        
.transition()
.duration(500)
.attr("r", 4)
.transition()
.duration(500)
.attr("r", 2)}
else{g.selectAll("circle.Chile").attr("opacity",.05)}

if (d.properties.name == "Thailand"){

g.selectAll("circle.Thailand").attr("opacity",0.75)        
.transition()
.duration(500)
.attr("r", 4)
.transition()
.duration(500)
.attr("r", 2)}
else{g.selectAll("circle.Thailand").attr("opacity",.05)}

})

var tooltip = d3.select("g")
.append("text")
.attr("fill-opacity", "0")
.text("tooltip!")
.classed("tooltip",true)
;

})

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
.attr("y", (d3.event.pageY)- 95 +"px")
.attr("x",(d3.event.pageX)- 100 +"px")
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



//make tooltip container
var tooltip = d3.select("g")
.append("text")
.attr("fill-opacity", "0")
.text("tooltip!")
.classed("tooltip", true);
})


//East Coast of North America
var ECminlat = 20;
var ECmaxlat = 40;
var ECminlon = -75;
var ECmaxlon = -55;

//South America
var SAMminlat = -40;
var SAMmaxlat = -30;
var SAMminlon = -70;
var SAMmaxlon = -50;

//West Coast of North America
var WCminlat = 20;
var WCmaxlat = 40;
var WCminlon = -145;
var WCmaxlon = -120;

//East Asia
var EAminlat = 20;
var EAmaxlat = 40;
var EAminlon = 110;
var EAmaxlon = 130;

//South East Asia
var SEAminlat = -10;
var SEAmaxlat = 10;
var SEAminlon = 130;
var SEAmaxlon = 150;


//South Asia
var SAminlat = 0;
var SAmaxlat = 20;
var SAminlon = 90;
var SAmaxlon = 110;


//Australia
var AUminlat = -30;
var AUmaxlat = -10;
var AUminlon = 135;
var AUmaxlon = 155;

//South Europe
var SEUminlat = 30;
var SEUmaxlat = 40;
var SEUminlon = 10;
var SEUmaxlon = 20;

//North Europe
var NEUminlat = 50;
var NEUmaxlat = 60;
var NEUminlon = 10;
var NEUmaxlon = 30;

//West Africa
var WFminlat = 20;
var WFmaxlat = 40;
var WFminlon = -30;
var WFmaxlon = -10;

//South Africa
var SFminlat = -30;
var SFmaxlat = -10;
var SFminlon = 20;
var SFmaxlon = 40;


d3.json("http://www.myshiptracking.com/requests/vesselsonmap.php?type=json&minlat=" + ECminlat + "&maxlat=" + ECmaxlat + "&minlon=" + ECminlon + "&maxlon=" + ECmaxlon + "&zoom=7", function(err, boats) {
console.log(boats)
g.selectAll("circle.boat")
.data(boats[0].DATA)
.enter()
.append("circle")
.attr("transform", function(d) {return "translate(" + projection([d.LNG, d.LAT]) + ")";})
.attr("r",function(d){if (d.TYPE == 10) {return 2} else {return 2} })
.attr("fill", function(d){if (d.TYPE == 10) {return "yellow"} else {return "#0ca"} })
.attr("opacity", .05)
.attr("stroke-width", .05)
.attr("stroke", "white")
.classed("boat",true)
.attr("class", function(d){var mmsi = d.MMSI;  var countrycode = mmsi.slice(0,3);

if ((countrycode == 338)||(countrycode == 303)||(countrycode >=366 && countrycode <= 369)) {return "USA" }
else{

if (countrycode == 316) {return "Canada" }
else{

if (countrycode == 345) {return "Mexico" }
else{

if ((countrycode == 412)||(countrycode == 413)||(countrycode == 414)||(countrycode == 453)||(countrycode == 477)) {return "China" }
else{ 

if (countrycode == 419) {return "India" }
else{
if ((countrycode == 431)||(countrycode == 432)) {return "Japan" }
else{
if ((countrycode == 440)||(countrycode == 441)) {return "Korea" }
else{
if (countrycode == 416) {return "Taiwan" }
else{
if (countrycode == 503) {return "Australia" }
else{
if ((countrycode == 232)||(countrycode == 233)||(countrycode == 234)||(countrycode == 235)){return "England" }
else{
if (countrycode == 710) {return "Brazil" }
else{
if (countrycode == 273) {return "Russia" }
else{
if ((countrycode == 211)||(countrycode == 218)) {return "Germany" }
else{
if ((countrycode == 226)||(countrycode == 227)||(countrycode == 228)){return "France" }
else{
if ((countrycode == 224)||(countrycode == 225)) {return "Spain" }
else{
if ((countrycode == 219)||(countrycode == 220)) {return "Denmark" }
else{
if ((countrycode == 239)||(countrycode == 240)||(countrycode == 241)){return "Greece" }
else{
if ((countrycode == 244)||(countrycode == 245)||(countrycode == 246)){return "Netherlands" }
else{
if ((countrycode == 257)||(countrycode == 258)||(countrycode == 259)) {return "Norway" }
else{
if ((countrycode == 265)||(countrycode == 266)) {return "Sweden" }
else{
if (countrycode == 574) {return "Vietnam" }
else{
if ((countrycode == 351)||(countrycode == 352)||(countrycode == 353)||(countrycode == 354)||(countrycode == 370)||(countrycode == 371)||(countrycode == 372)){return "Panama" }
else{
if (countrycode == 601) {return "Africa" }
else{
if (countrycode == 760) {return "Peru" }
else{
if (countrycode == 525) {return "Indonesia" }
else{
if (countrycode == 725) {return "Chile" }
else{
if (countrycode == 567) {return "Thailand" }
else{return "unknown origin"

} 
} 
} 
} 
} 
} 
} 
} 
} 
} 
} 
} 
} 
} 
} 
} 
} 
} 
} 
} 
} 
} 
}  
}
}
}
}
})
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



d3.json("http://www.myshiptracking.com/requests/vesselsonmap.php?type=json&minlat=" + SAMminlat + "&maxlat=" + SAMmaxlat + "&minlon=" + SAMminlon + "&maxlon=" + SAMmaxlon + "&zoom=7", function(err, boats) {
console.log(boats)
g.selectAll("circle.boat")
.data(boats[0].DATA)
.enter()
.append("circle")
.attr("transform", function(d) {return "translate(" + projection([d.LNG, d.LAT]) + ")";})
.attr("r",function(d){if (d.TYPE == 10) {return 2} else {return 2} })
.attr("fill", function(d){if (d.TYPE == 10) {return "yellow"} else {return "#0ca"} })
.attr("opacity", .05)
.attr("stroke-width", .05)
.attr("stroke", "white")
.classed("boat",true)
.attr("class", function(d){var mmsi = d.MMSI;  var countrycode = mmsi.slice(0,3);

if ((countrycode == 338)||(countrycode == 303)||(countrycode >=366 && countrycode <= 369)) {return "USA" }
else{

if (countrycode == 316) {return "Canada" }
else{

if (countrycode == 345) {return "Mexico" }
else{

if ((countrycode == 412)||(countrycode == 413)||(countrycode == 414)||(countrycode == 453)||(countrycode == 477)) {return "China" }
else{ 

if (countrycode == 419) {return "India" }
else{
if ((countrycode == 431)||(countrycode == 432)) {return "Japan" }
else{
if ((countrycode == 440)||(countrycode == 441)) {return "Korea" }
else{
if (countrycode == 416) {return "Taiwan" }
else{
if (countrycode == 503) {return "Australia" }
else{
if ((countrycode == 232)||(countrycode == 233)||(countrycode == 234)||(countrycode == 235)){return "England" }
else{
if (countrycode == 710) {return "Brazil" }
else{
if (countrycode == 273) {return "Russia" }
else{
if ((countrycode == 211)||(countrycode == 218)) {return "Germany" }
else{
if ((countrycode == 226)||(countrycode == 227)||(countrycode == 228)){return "France" }
else{
if ((countrycode == 224)||(countrycode == 225)) {return "Spain" }
else{
if ((countrycode == 219)||(countrycode == 220)) {return "Denmark" }
else{
if ((countrycode == 239)||(countrycode == 240)||(countrycode == 241)){return "Greece" }
else{
if ((countrycode == 244)||(countrycode == 245)||(countrycode == 246)){return "Netherlands" }
else{
if ((countrycode == 257)||(countrycode == 258)||(countrycode == 259)) {return "Norway" }
else{
if ((countrycode == 265)||(countrycode == 266)) {return "Sweden" }
else{
if (countrycode == 574) {return "Vietnam" }
else{
if ((countrycode == 351)||(countrycode == 352)||(countrycode == 353)||(countrycode == 354)||(countrycode == 370)||(countrycode == 371)||(countrycode == 372)){return "Panama" }
else{
if (countrycode == 601) {return "Africa" }
else{
if (countrycode == 760) {return "Peru" }
else{
if (countrycode == 525) {return "Indonesia" }
else{
if (countrycode == 725) {return "Chile" }
else{
if (countrycode == 567) {return "Thailand" }
else{return "unknown origin"

} 
} 
} 
} 
} 
} 
} 
} 
} 
} 
} 
} 
} 
} 
} 
} 
} 
} 
} 
} 
} 
} 
}  
}
}
}
}
})
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


d3.json("http://www.myshiptracking.com/requests/vesselsonmap.php?type=json&minlat=" + WCminlat + "&maxlat=" + WCmaxlat + "&minlon=" + WCminlon + "&maxlon=" + WCmaxlon + "&zoom=7", function(err, boats) {
console.log(boats)
g.selectAll("circle.boat")
.data(boats[0].DATA)
.enter()
.append("circle")
.attr("transform", function(d) {return "translate(" + projection([d.LNG, d.LAT]) + ")";})
.attr("r",function(d){if (d.TYPE == 10) {return 2} else {return 2} })
.attr("fill", function(d){if (d.TYPE == 10) {return "yellow"} else {return "#0ca"} })
.attr("opacity", .05)
.attr("stroke-width", .05)
.attr("stroke", "white")
.classed("boat",true)
.attr("class", function(d){var mmsi = d.MMSI;  var countrycode = mmsi.slice(0,3);

if ((countrycode == 338)||(countrycode == 303)||(countrycode >=366 && countrycode <= 369)) {return "USA" }
else{

if (countrycode == 316) {return "Canada" }
else{

if (countrycode == 345) {return "Mexico" }
else{

if ((countrycode == 412)||(countrycode == 413)||(countrycode == 414)||(countrycode == 453)||(countrycode == 477)) {return "China" }
else{ 

if (countrycode == 419) {return "India" }
else{
if ((countrycode == 431)||(countrycode == 432)) {return "Japan" }
else{
if ((countrycode == 440)||(countrycode == 441)) {return "Korea" }
else{
if (countrycode == 416) {return "Taiwan" }
else{
if (countrycode == 503) {return "Australia" }
else{
if ((countrycode == 232)||(countrycode == 233)||(countrycode == 234)||(countrycode == 235)){return "England" }
else{
if (countrycode == 710) {return "Brazil" }
else{
if (countrycode == 273) {return "Russia" }
else{
if ((countrycode == 211)||(countrycode == 218)) {return "Germany" }
else{
if ((countrycode == 226)||(countrycode == 227)||(countrycode == 228)){return "France" }
else{
if ((countrycode == 224)||(countrycode == 225)) {return "Spain" }
else{
if ((countrycode == 219)||(countrycode == 220)) {return "Denmark" }
else{
if ((countrycode == 239)||(countrycode == 240)||(countrycode == 241)){return "Greece" }
else{
if ((countrycode == 244)||(countrycode == 245)||(countrycode == 246)){return "Netherlands" }
else{
if ((countrycode == 257)||(countrycode == 258)||(countrycode == 259)) {return "Norway" }
else{
if ((countrycode == 265)||(countrycode == 266)) {return "Sweden" }
else{
if (countrycode == 574) {return "Vietnam" }
else{
if ((countrycode == 351)||(countrycode == 352)||(countrycode == 353)||(countrycode == 354)||(countrycode == 370)||(countrycode == 371)||(countrycode == 372)){return "Panama" }
else{
if (countrycode == 601) {return "Africa" }
else{
if (countrycode == 760) {return "Peru" }
else{
if (countrycode == 525) {return "Indonesia" }
else{
if (countrycode == 725) {return "Chile" }
else{
if (countrycode == 567) {return "Thailand" }
else{return "unknown origin"

} 
} 
} 
} 
} 
} 
} 
} 
} 
} 
} 
} 
} 
} 
} 
} 
} 
} 
} 
} 
} 
} 
}  
}
}
}
}
})
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


d3.json("http://www.myshiptracking.com/requests/vesselsonmap.php?type=json&minlat=" + EAminlat + "&maxlat=" + EAmaxlat + "&minlon=" + EAminlon + "&maxlon=" + EAmaxlon + "&zoom=7", function(err, boats) {
console.log(boats)
g.selectAll("circle.boat")
.data(boats[0].DATA)
.enter()
.append("circle")
.attr("transform", function(d) {return "translate(" + projection([d.LNG, d.LAT]) + ")";})
.attr("r",function(d){if (d.TYPE == 10) {return 2} else {return 2} })
.attr("fill", function(d){if (d.TYPE == 10) {return "yellow"} else {return "#0ca"} })
.attr("opacity", .05)
.attr("stroke-width", .05)
.attr("stroke", "white")
.classed("boat",true)
.attr("class", function(d){var mmsi = d.MMSI;  var countrycode = mmsi.slice(0,3);

if ((countrycode == 338)||(countrycode == 303)||(countrycode >=366 && countrycode <= 369)) {return "USA" }
else{

if (countrycode == 316) {return "Canada" }
else{

if (countrycode == 345) {return "Mexico" }
else{

if ((countrycode == 412)||(countrycode == 413)||(countrycode == 414)||(countrycode == 453)||(countrycode == 477)) {return "China" }
else{ 

if (countrycode == 419) {return "India" }
else{
if ((countrycode == 431)||(countrycode == 432)) {return "Japan" }
else{
if ((countrycode == 440)||(countrycode == 441)) {return "Korea" }
else{
if (countrycode == 416) {return "Taiwan" }
else{
if (countrycode == 503) {return "Australia" }
else{
if ((countrycode == 232)||(countrycode == 233)||(countrycode == 234)||(countrycode == 235)){return "England" }
else{
if (countrycode == 710) {return "Brazil" }
else{
if (countrycode == 273) {return "Russia" }
else{
if ((countrycode == 211)||(countrycode == 218)) {return "Germany" }
else{
if ((countrycode == 226)||(countrycode == 227)||(countrycode == 228)){return "France" }
else{
if ((countrycode == 224)||(countrycode == 225)) {return "Spain" }
else{
if ((countrycode == 219)||(countrycode == 220)) {return "Denmark" }
else{
if ((countrycode == 239)||(countrycode == 240)||(countrycode == 241)){return "Greece" }
else{
if ((countrycode == 244)||(countrycode == 245)||(countrycode == 246)){return "Netherlands" }
else{
if ((countrycode == 257)||(countrycode == 258)||(countrycode == 259)) {return "Norway" }
else{
if ((countrycode == 265)||(countrycode == 266)) {return "Sweden" }
else{
if (countrycode == 574) {return "Vietnam" }
else{
if ((countrycode == 351)||(countrycode == 352)||(countrycode == 353)||(countrycode == 354)||(countrycode == 370)||(countrycode == 371)||(countrycode == 372)){return "Panama" }
else{
if (countrycode == 601) {return "Africa" }
else{
if (countrycode == 760) {return "Peru" }
else{
if (countrycode == 525) {return "Indonesia" }
else{
if (countrycode == 725) {return "Chile" }
else{
if (countrycode == 567) {return "Thailand" }
else{return "unknown origin"

} 
} 
} 
} 
} 
} 
} 
} 
} 
} 
} 
} 
} 
} 
} 
} 
} 
} 
} 
} 
} 
} 
}  
}
}
}
}
})
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

d3.json("http://www.myshiptracking.com/requests/vesselsonmap.php?type=json&minlat=" + SEAminlat + "&maxlat=" + SEAmaxlat + "&minlon=" + SEAminlon + "&maxlon=" + SEAmaxlon + "&zoom=7", function(err, boats) {
console.log(boats)
g.selectAll("circle.boat")
.data(boats[0].DATA)
.enter()
.append("circle")
.attr("transform", function(d) {return "translate(" + projection([d.LNG, d.LAT]) + ")";})
.attr("r",function(d){if (d.TYPE == 10) {return 2} else {return 2} })
.attr("fill", function(d){if (d.TYPE == 10) {return "yellow"} else {return "#0ca"} })
.attr("opacity", .05)
.attr("stroke-width", .05)
.attr("stroke", "white")
.classed("boat",true)
.attr("class", function(d){var mmsi = d.MMSI;  var countrycode = mmsi.slice(0,3);

if ((countrycode == 338)||(countrycode == 303)||(countrycode >=366 && countrycode <= 369)) {return "USA" }
else{

if (countrycode == 316) {return "Canada" }
else{

if (countrycode == 345) {return "Mexico" }
else{

if ((countrycode == 412)||(countrycode == 413)||(countrycode == 414)||(countrycode == 453)||(countrycode == 477)) {return "China" }
else{ 

if (countrycode == 419) {return "India" }
else{
if ((countrycode == 431)||(countrycode == 432)) {return "Japan" }
else{
if ((countrycode == 440)||(countrycode == 441)) {return "Korea" }
else{
if (countrycode == 416) {return "Taiwan" }
else{
if (countrycode == 503) {return "Australia" }
else{
if ((countrycode == 232)||(countrycode == 233)||(countrycode == 234)||(countrycode == 235)){return "England" }
else{
if (countrycode == 710) {return "Brazil" }
else{
if (countrycode == 273) {return "Russia" }
else{
if ((countrycode == 211)||(countrycode == 218)) {return "Germany" }
else{
if ((countrycode == 226)||(countrycode == 227)||(countrycode == 228)){return "France" }
else{
if ((countrycode == 224)||(countrycode == 225)) {return "Spain" }
else{
if ((countrycode == 219)||(countrycode == 220)) {return "Denmark" }
else{
if ((countrycode == 239)||(countrycode == 240)||(countrycode == 241)){return "Greece" }
else{
if ((countrycode == 244)||(countrycode == 245)||(countrycode == 246)){return "Netherlands" }
else{
if ((countrycode == 257)||(countrycode == 258)||(countrycode == 259)) {return "Norway" }
else{
if ((countrycode == 265)||(countrycode == 266)) {return "Sweden" }
else{
if (countrycode == 574) {return "Vietnam" }
else{
if ((countrycode == 351)||(countrycode == 352)||(countrycode == 353)||(countrycode == 354)||(countrycode == 370)||(countrycode == 371)||(countrycode == 372)){return "Panama" }
else{
if (countrycode == 601) {return "Africa" }
else{
if (countrycode == 760) {return "Peru" }
else{
if (countrycode == 525) {return "Indonesia" }
else{
if (countrycode == 725) {return "Chile" }
else{
if (countrycode == 567) {return "Thailand" }
else{return "unknown origin"

} 
} 
} 
} 
} 
} 
} 
} 
} 
} 
} 
} 
} 
} 
} 
} 
} 
} 
} 
} 
} 
} 
}  
}
}
}
}
})
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

d3.json("http://www.myshiptracking.com/requests/vesselsonmap.php?type=json&minlat=" + SAminlat + "&maxlat=" + SAmaxlat + "&minlon=" + SAminlon + "&maxlon=" + SAmaxlon + "&zoom=7", function(err, boats) {
console.log(boats)
g.selectAll("circle.boat")
.data(boats[0].DATA)
.enter()
.append("circle")
.attr("transform", function(d) {return "translate(" + projection([d.LNG, d.LAT]) + ")";})
.attr("r",function(d){if (d.TYPE == 10) {return 2} else {return 2} })
.attr("fill", function(d){if (d.TYPE == 10) {return "yellow"} else {return "#0ca"} })
.attr("opacity", .05)
.attr("stroke-width", .05)
.attr("stroke", "white")
.classed("boat",true)
.attr("class", function(d){var mmsi = d.MMSI;  var countrycode = mmsi.slice(0,3);

if ((countrycode == 338)||(countrycode == 303)||(countrycode >=366 && countrycode <= 369)) {return "USA" }
else{

if (countrycode == 316) {return "Canada" }
else{

if (countrycode == 345) {return "Mexico" }
else{

if ((countrycode == 412)||(countrycode == 413)||(countrycode == 414)||(countrycode == 453)||(countrycode == 477)) {return "China" }
else{ 

if (countrycode == 419) {return "India" }
else{
if ((countrycode == 431)||(countrycode == 432)) {return "Japan" }
else{
if ((countrycode == 440)||(countrycode == 441)) {return "Korea" }
else{
if (countrycode == 416) {return "Taiwan" }
else{
if (countrycode == 503) {return "Australia" }
else{
if ((countrycode == 232)||(countrycode == 233)||(countrycode == 234)||(countrycode == 235)){return "England" }
else{
if (countrycode == 710) {return "Brazil" }
else{
if (countrycode == 273) {return "Russia" }
else{
if ((countrycode == 211)||(countrycode == 218)) {return "Germany" }
else{
if ((countrycode == 226)||(countrycode == 227)||(countrycode == 228)){return "France" }
else{
if ((countrycode == 224)||(countrycode == 225)) {return "Spain" }
else{
if ((countrycode == 219)||(countrycode == 220)) {return "Denmark" }
else{
if ((countrycode == 239)||(countrycode == 240)||(countrycode == 241)){return "Greece" }
else{
if ((countrycode == 244)||(countrycode == 245)||(countrycode == 246)){return "Netherlands" }
else{
if ((countrycode == 257)||(countrycode == 258)||(countrycode == 259)) {return "Norway" }
else{
if ((countrycode == 265)||(countrycode == 266)) {return "Sweden" }
else{
if (countrycode == 574) {return "Vietnam" }
else{
if ((countrycode == 351)||(countrycode == 352)||(countrycode == 353)||(countrycode == 354)||(countrycode == 370)||(countrycode == 371)||(countrycode == 372)){return "Panama" }
else{
if (countrycode == 601) {return "Africa" }
else{
if (countrycode == 760) {return "Peru" }
else{
if (countrycode == 525) {return "Indonesia" }
else{
if (countrycode == 725) {return "Chile" }
else{
if (countrycode == 567) {return "Thailand" }
else{return "unknown origin"

} 
} 
} 
} 
} 
} 
} 
} 
} 
} 
} 
} 
} 
} 
} 
} 
} 
} 
} 
} 
} 
} 
}  
}
}
}
}
})
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

d3.json("http://www.myshiptracking.com/requests/vesselsonmap.php?type=json&minlat=" + AUminlat + "&maxlat=" + AUmaxlat + "&minlon=" + AUminlon + "&maxlon=" + AUmaxlon + "&zoom=7", function(err, boats) {
console.log(boats)
g.selectAll("circle.boat")
.data(boats[0].DATA)
.enter()
.append("circle")
.attr("transform", function(d) {return "translate(" + projection([d.LNG, d.LAT]) + ")";})
.attr("r",function(d){if (d.TYPE == 10) {return 2} else {return 2} })
.attr("fill", function(d){if (d.TYPE == 10) {return "yellow"} else {return "#0ca"} })
.attr("opacity", .05)
.attr("stroke-width", .05)
.attr("stroke", "white")
.classed("boat",true)
.attr("class", function(d){var mmsi = d.MMSI;  var countrycode = mmsi.slice(0,3);

if ((countrycode == 338)||(countrycode == 303)||(countrycode >=366 && countrycode <= 369)) {return "USA" }
else{

if (countrycode == 316) {return "Canada" }
else{

if (countrycode == 345) {return "Mexico" }
else{

if ((countrycode == 412)||(countrycode == 413)||(countrycode == 414)||(countrycode == 453)||(countrycode == 477)) {return "China" }
else{ 

if (countrycode == 419) {return "India" }
else{
if ((countrycode == 431)||(countrycode == 432)) {return "Japan" }
else{
if ((countrycode == 440)||(countrycode == 441)) {return "Korea" }
else{
if (countrycode == 416) {return "Taiwan" }
else{
if (countrycode == 503) {return "Australia" }
else{
if ((countrycode == 232)||(countrycode == 233)||(countrycode == 234)||(countrycode == 235)){return "England" }
else{
if (countrycode == 710) {return "Brazil" }
else{
if (countrycode == 273) {return "Russia" }
else{
if ((countrycode == 211)||(countrycode == 218)) {return "Germany" }
else{
if ((countrycode == 226)||(countrycode == 227)||(countrycode == 228)){return "France" }
else{
if ((countrycode == 224)||(countrycode == 225)) {return "Spain" }
else{
if ((countrycode == 219)||(countrycode == 220)) {return "Denmark" }
else{
if ((countrycode == 239)||(countrycode == 240)||(countrycode == 241)){return "Greece" }
else{
if ((countrycode == 244)||(countrycode == 245)||(countrycode == 246)){return "Netherlands" }
else{
if ((countrycode == 257)||(countrycode == 258)||(countrycode == 259)) {return "Norway" }
else{
if ((countrycode == 265)||(countrycode == 266)) {return "Sweden" }
else{
if (countrycode == 574) {return "Vietnam" }
else{
if ((countrycode == 351)||(countrycode == 352)||(countrycode == 353)||(countrycode == 354)||(countrycode == 370)||(countrycode == 371)||(countrycode == 372)){return "Panama" }
else{
if (countrycode == 601) {return "Africa" }
else{
if (countrycode == 760) {return "Peru" }
else{
if (countrycode == 525) {return "Indonesia" }
else{
if (countrycode == 725) {return "Chile" }
else{
if (countrycode == 567) {return "Thailand" }
else{return "unknown origin"

} 
} 
} 
} 
} 
} 
} 
} 
} 
} 
} 
} 
} 
} 
} 
} 
} 
} 
} 
} 
} 
} 
}  
}
}
}
}
})
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


d3.json("http://www.myshiptracking.com/requests/vesselsonmap.php?type=json&minlat=" + SEUminlat + "&maxlat=" + SEUmaxlat + "&minlon=" + SEUminlon + "&maxlon=" + SEUmaxlon + "&zoom=7", function(err, boats) {
console.log(boats)
g.selectAll("circle.boat")
.data(boats[0].DATA)
.enter()
.append("circle")
.attr("transform", function(d) {return "translate(" + projection([d.LNG, d.LAT]) + ")";})
.attr("r",function(d){if (d.TYPE == 10) {return 2} else {return 2} })
.attr("fill", function(d){if (d.TYPE == 10) {return "yellow"} else {return "#0ca"} })
.attr("opacity", .05)
.attr("stroke-width", .05)
.attr("stroke", "white")
.classed("boat",true)
.attr("class", function(d){var mmsi = d.MMSI;  var countrycode = mmsi.slice(0,3);

if ((countrycode == 338)||(countrycode == 303)||(countrycode >=366 && countrycode <= 369)) {return "USA" }
else{

if (countrycode == 316) {return "Canada" }
else{

if (countrycode == 345) {return "Mexico" }
else{

if ((countrycode == 412)||(countrycode == 413)||(countrycode == 414)||(countrycode == 453)||(countrycode == 477)) {return "China" }
else{ 

if (countrycode == 419) {return "India" }
else{
if ((countrycode == 431)||(countrycode == 432)) {return "Japan" }
else{
if ((countrycode == 440)||(countrycode == 441)) {return "Korea" }
else{
if (countrycode == 416) {return "Taiwan" }
else{
if (countrycode == 503) {return "Australia" }
else{
if ((countrycode == 232)||(countrycode == 233)||(countrycode == 234)||(countrycode == 235)){return "England" }
else{
if (countrycode == 710) {return "Brazil" }
else{
if (countrycode == 273) {return "Russia" }
else{
if ((countrycode == 211)||(countrycode == 218)) {return "Germany" }
else{
if ((countrycode == 226)||(countrycode == 227)||(countrycode == 228)){return "France" }
else{
if ((countrycode == 224)||(countrycode == 225)) {return "Spain" }
else{
if ((countrycode == 219)||(countrycode == 220)) {return "Denmark" }
else{
if ((countrycode == 239)||(countrycode == 240)||(countrycode == 241)){return "Greece" }
else{
if ((countrycode == 244)||(countrycode == 245)||(countrycode == 246)){return "Netherlands" }
else{
if ((countrycode == 257)||(countrycode == 258)||(countrycode == 259)) {return "Norway" }
else{
if ((countrycode == 265)||(countrycode == 266)) {return "Sweden" }
else{
if (countrycode == 574) {return "Vietnam" }
else{
if ((countrycode == 351)||(countrycode == 352)||(countrycode == 353)||(countrycode == 354)||(countrycode == 370)||(countrycode == 371)||(countrycode == 372)){return "Panama" }
else{
if (countrycode == 601) {return "Africa" }
else{
if (countrycode == 760) {return "Peru" }
else{
if (countrycode == 525) {return "Indonesia" }
else{
if (countrycode == 725) {return "Chile" }
else{
if (countrycode == 567) {return "Thailand" }
else{return "unknown origin"

} 
} 
} 
} 
} 
} 
} 
} 
} 
} 
} 
} 
} 
} 
} 
} 
} 
} 
} 
} 
} 
} 
}  
}
}
}
}
})
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

d3.json("http://www.myshiptracking.com/requests/vesselsonmap.php?type=json&minlat=" + NEUminlat + "&maxlat=" + NEUmaxlat + "&minlon=" + NEUminlon + "&maxlon=" + NEUmaxlon + "&zoom=7", function(err, boats) {
console.log(boats)
g.selectAll("circle.boat")
.data(boats[0].DATA)
.enter()
.append("circle")
.attr("transform", function(d) {return "translate(" + projection([d.LNG, d.LAT]) + ")";})
.attr("r",function(d){if (d.TYPE == 10) {return 2} else {return 2} })
.attr("fill", function(d){if (d.TYPE == 10) {return "yellow"} else {return "#0ca"} })
.attr("opacity", .05)
.attr("stroke-width", .05)
.attr("stroke", "white")
.classed("boat",true)
.attr("class", function(d){var mmsi = d.MMSI;  var countrycode = mmsi.slice(0,3);

if ((countrycode == 338)||(countrycode == 303)||(countrycode >=366 && countrycode <= 369)) {return "USA" }
else{

if (countrycode == 316) {return "Canada" }
else{

if (countrycode == 345) {return "Mexico" }
else{

if ((countrycode == 412)||(countrycode == 413)||(countrycode == 414)||(countrycode == 453)||(countrycode == 477)) {return "China" }
else{ 

if (countrycode == 419) {return "India" }
else{
if ((countrycode == 431)||(countrycode == 432)) {return "Japan" }
else{
if ((countrycode == 440)||(countrycode == 441)) {return "Korea" }
else{
if (countrycode == 416) {return "Taiwan" }
else{
if (countrycode == 503) {return "Australia" }
else{
if ((countrycode == 232)||(countrycode == 233)||(countrycode == 234)||(countrycode == 235)){return "England" }
else{
if (countrycode == 710) {return "Brazil" }
else{
if (countrycode == 273) {return "Russia" }
else{
if ((countrycode == 211)||(countrycode == 218)) {return "Germany" }
else{
if ((countrycode == 226)||(countrycode == 227)||(countrycode == 228)){return "France" }
else{
if ((countrycode == 224)||(countrycode == 225)) {return "Spain" }
else{
if ((countrycode == 219)||(countrycode == 220)) {return "Denmark" }
else{
if ((countrycode == 239)||(countrycode == 240)||(countrycode == 241)){return "Greece" }
else{
if ((countrycode == 244)||(countrycode == 245)||(countrycode == 246)){return "Netherlands" }
else{
if ((countrycode == 257)||(countrycode == 258)||(countrycode == 259)) {return "Norway" }
else{
if ((countrycode == 265)||(countrycode == 266)) {return "Sweden" }
else{
if (countrycode == 574) {return "Vietnam" }
else{
if ((countrycode == 351)||(countrycode == 352)||(countrycode == 353)||(countrycode == 354)||(countrycode == 370)||(countrycode == 371)||(countrycode == 372)){return "Panama" }
else{
if (countrycode == 601) {return "Africa" }
else{
if (countrycode == 760) {return "Peru" }
else{
if (countrycode == 525) {return "Indonesia" }
else{
if (countrycode == 725) {return "Chile" }
else{
if (countrycode == 567) {return "Thailand" }
else{return "unknown origin"

} 
} 
} 
} 
} 
} 
} 
} 
} 
} 
} 
} 
} 
} 
} 
} 
} 
} 
} 
} 
} 
} 
}  
}
}
}
}
})
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

d3.json("http://www.myshiptracking.com/requests/vesselsonmap.php?type=json&minlat=" + WFminlat + "&maxlat=" + WFmaxlat + "&minlon=" + WFminlon + "&maxlon=" + WFmaxlon + "&zoom=7", function(err, boats) {
console.log(boats)
g.selectAll("circle.boat")
.data(boats[0].DATA)
.enter()
.append("circle")
.attr("transform", function(d) {return "translate(" + projection([d.LNG, d.LAT]) + ")";})
.attr("r",function(d){if (d.TYPE == 10) {return 2} else {return 2} })
.attr("fill", function(d){if (d.TYPE == 10) {return "yellow"} else {return "#0ca"} })
.attr("opacity", .05)
.attr("stroke-width", .05)
.attr("stroke", "white")
.classed("boat",true)
.attr("class", function(d){var mmsi = d.MMSI;  var countrycode = mmsi.slice(0,3);

if ((countrycode == 338)||(countrycode == 303)||(countrycode >=366 && countrycode <= 369)) {return "USA" }
else{

if (countrycode == 316) {return "Canada" }
else{

if (countrycode == 345) {return "Mexico" }
else{

if ((countrycode == 412)||(countrycode == 413)||(countrycode == 414)||(countrycode == 453)||(countrycode == 477)) {return "China" }
else{ 

if (countrycode == 419) {return "India" }
else{
if ((countrycode == 431)||(countrycode == 432)) {return "Japan" }
else{
if ((countrycode == 440)||(countrycode == 441)) {return "Korea" }
else{
if (countrycode == 416) {return "Taiwan" }
else{
if (countrycode == 503) {return "Australia" }
else{
if ((countrycode == 232)||(countrycode == 233)||(countrycode == 234)||(countrycode == 235)){return "England" }
else{
if (countrycode == 710) {return "Brazil" }
else{
if (countrycode == 273) {return "Russia" }
else{
if ((countrycode == 211)||(countrycode == 218)) {return "Germany" }
else{
if ((countrycode == 226)||(countrycode == 227)||(countrycode == 228)){return "France" }
else{
if ((countrycode == 224)||(countrycode == 225)) {return "Spain" }
else{
if ((countrycode == 219)||(countrycode == 220)) {return "Denmark" }
else{
if ((countrycode == 239)||(countrycode == 240)||(countrycode == 241)){return "Greece" }
else{
if ((countrycode == 244)||(countrycode == 245)||(countrycode == 246)){return "Netherlands" }
else{
if ((countrycode == 257)||(countrycode == 258)||(countrycode == 259)) {return "Norway" }
else{
if ((countrycode == 265)||(countrycode == 266)) {return "Sweden" }
else{
if (countrycode == 574) {return "Vietnam" }
else{
if ((countrycode == 351)||(countrycode == 352)||(countrycode == 353)||(countrycode == 354)||(countrycode == 370)||(countrycode == 371)||(countrycode == 372)){return "Panama" }
else{
if (countrycode == 601) {return "Africa" }
else{
if (countrycode == 760) {return "Peru" }
else{
if (countrycode == 525) {return "Indonesia" }
else{
if (countrycode == 725) {return "Chile" }
else{
if (countrycode == 567) {return "Thailand" }
else{return "unknown origin"

} 
} 
} 
} 
} 
} 
} 
} 
} 
} 
} 
} 
} 
} 
} 
} 
} 
} 
} 
} 
} 
} 
}  
}
}
}
}
})
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


d3.json("http://www.myshiptracking.com/requests/vesselsonmap.php?type=json&minlat=" + SFminlat + "&maxlat=" + SFmaxlat + "&minlon=" + SFminlon + "&maxlon=" + SFmaxlon + "&zoom=7", function(err, boats) {
console.log(boats)
g.selectAll("circle.boat")
.data(boats[0].DATA)
.enter()
.append("circle")
.attr("transform", function(d) {return "translate(" + projection([d.LNG, d.LAT]) + ")";})
.attr("r",function(d){if (d.TYPE == 10) {return 2} else {return 2} })
.attr("fill", function(d){if (d.TYPE == 10) {return "yellow"} else {return "#0ca"} })
.attr("opacity", .05)
.attr("stroke-width", .05)
.attr("stroke", "white")
.classed("boat",true)
.attr("class", function(d){var mmsi = d.MMSI;  var countrycode = mmsi.slice(0,3);

if ((countrycode == 338)||(countrycode == 303)||(countrycode >=366 && countrycode <= 369)) {return "USA" }
else{

if (countrycode == 316) {return "Canada" }
else{

if (countrycode == 345) {return "Mexico" }
else{

if ((countrycode == 412)||(countrycode == 413)||(countrycode == 414)||(countrycode == 453)||(countrycode == 477)) {return "China" }
else{ 

if (countrycode == 419) {return "India" }
else{
if ((countrycode == 431)||(countrycode == 432)) {return "Japan" }
else{
if ((countrycode == 440)||(countrycode == 441)) {return "Korea" }
else{
if (countrycode == 416) {return "Taiwan" }
else{
if (countrycode == 503) {return "Australia" }
else{
if ((countrycode == 232)||(countrycode == 233)||(countrycode == 234)||(countrycode == 235)){return "England" }
else{
if (countrycode == 710) {return "Brazil" }
else{
if (countrycode == 273) {return "Russia" }
else{
if ((countrycode == 211)||(countrycode == 218)) {return "Germany" }
else{
if ((countrycode == 226)||(countrycode == 227)||(countrycode == 228)){return "France" }
else{
if ((countrycode == 224)||(countrycode == 225)) {return "Spain" }
else{
if ((countrycode == 219)||(countrycode == 220)) {return "Denmark" }
else{
if ((countrycode == 239)||(countrycode == 240)||(countrycode == 241)){return "Greece" }
else{
if ((countrycode == 244)||(countrycode == 245)||(countrycode == 246)){return "Netherlands" }
else{
if ((countrycode == 257)||(countrycode == 258)||(countrycode == 259)) {return "Norway" }
else{
if ((countrycode == 265)||(countrycode == 266)) {return "Sweden" }
else{
if (countrycode == 574) {return "Vietnam" }
else{
if ((countrycode == 351)||(countrycode == 352)||(countrycode == 353)||(countrycode == 354)||(countrycode == 370)||(countrycode == 371)||(countrycode == 372)){return "Panama" }
else{
if (countrycode == 601) {return "Africa" }
else{
if (countrycode == 760) {return "Peru" }
else{
if (countrycode == 525) {return "Indonesia" }
else{
if (countrycode == 725) {return "Chile" }
else{
if (countrycode == 567) {return "Thailand" }
else{return "unknown origin"

} 
} 
} 
} 
} 
} 
} 
} 
} 
} 
} 
} 
} 
} 
} 
} 
} 
} 
} 
} 
} 
} 
}  
}
}
}
}
})
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

var graticule = d3.geoGraticule().step([10,10]);
g.append("path")
.datum(graticule)
.attr("d", path)
.classed("grat", true)
;


function zoomed() {
g.attr("transform", d3.event.transform);
}

</script>
</div>
</div>

</body>
</html>
```

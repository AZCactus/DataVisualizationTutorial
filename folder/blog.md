## Upload Blog

**Basic of HTML and CSS**
- [Standing Up a Server](https://github.com/zachpino/id_proto/tree/master/week1)
- [HTML Elements and the Box Model](https://github.com/zachpino/id_proto/tree/master/week2)
- [CSS Positioning](https://github.com/zachpino/id_proto/tree/master/week3)
- [Responsive Design](https://github.com/zachpino/id_proto/tree/master/week4)

### MY CODES
```
<html>
<head>
  <title>Post</title>

  <link href="reset.css" rel="stylesheet">

  <style>

    .label{font-family:'menlo';fill: #fff;}
    .country {fill:#2A4D65; stroke: #1C3148; stroke-width:.2px;}
    #sphere{fill:#1C3148; stroke: #fff; stroke-width:2px;}
    .grat {fill:none; stroke: #fff; stroke-width:.1px;}

    html {
      box-sizing: border-box;
    }

    *, *:before, *:after {
      box-sizing: inherit;
    }

    #container{
      width:100%;
      margin-left: auto;
      margin-right: auto;
    }

    #containermain{
      width:100%;
      float: left;
    }

    .row::after {
      content: "";
      clear: both;
      display: table;
    }

    .col{           
      float:left;
      min-height:50px;
      margin-left:.5%;
      margin-right:.5%;
      margin-top:.5%;
      margin-bottom:.5%;
    }

    .centered{
      margin-left:auto;
      margin-right:auto;
      float:none;
    }

    .one{ width:7.333%; }
    .two{ width:15.666%; }
    .three{ width:24%; }
    .four{ width:32.333%; }
    .five{ width:40.666% }
    .six{ width:49%; }
    .seven{ width:57.333%; }
    .eight{ width:65.666%; }
    .nine{ width:74%; }
    .ten{ width:82.333%; }
    .eleven{ width:90.666%; }
    .twelve{ width:99%; }


    /*Standard Mobile Style: Half or Full Widths*/
    @media only screen 
    and (min-width : 320px) 
    and (max-width : 479px) {
      #container
      .one { display:none; }
      .two { display:none; }
      .three { display:none }
      .four { display:none}
      .five { display:none }
      .six { width:49%; }
      .seven { width:99%; }
      .eight { width:99%; }
      .nine { width:99%; }
      .ten { width:99%; }
      .eleven { width:99%; }
      .twelve { width:99%; }
    }

    /*Large Mobile Styles: Full, Half, Third Widths*/
    @media only screen 
    and (min-width : 480px) 
    and (max-width : 767px) {
      #container
      .one { display:none; }
      .two { display:none; }
      .three { display:none }
      .four { width:32.333%}
      .five { width:49% }
      .six { width:49%; }
      .seven { width:49%; }
      .eight { width:65.666%; }
      .nine { width:99%; }
      .ten { width:99%; }
      .eleven { width:99%; }
      .twelve { width:99%; }
    }


    /*Tablets: Full, Half, Third, Quarter Widths*/
    @media only screen 
    and (min-width : 768px) 
    and (max-width : 1023px) {
     #container
     .one { width:24%; }
     .two { width:24%; }
     .three { width:24%; }
     .four { width:32.333%; }
     .five { width:49% }
     .six { width:49%; }
     .seven { width:49%; }
     .eight { width:65.666%; }
     .nine { width:74%; }
     .ten { width:74%; }
     .eleven { width:74%; }
     .twelve { width:99%; }
   }

   /* Laptops: All 12 Widths */
   @media only screen 
   and (min-width : 1024px) 
   and (max-width : 1223px) {
     #container
     .one { width:15.666%; }
     .two { width:15.666%; }
     .three { width:24% }
     .four { width:32.333%}
     .five { width:40.666% }
     .six { width:49%; }
     .seven { width:57.333%; }
     .eight { width:65.666%; }
     .nine { width:74%; }
     .ten { width:82.333%; }
     .eleven { width:82.333%; }
     .twelve { width:99%; }
   }

   /*Desktop: All 12 Widths*/
   @media only screen and (min-width : 1224px) {

    #container{ 
      width:1224px;
    }
  }

</style>
</head>

<body>
  <script src="https://d3js.org/d3.v4.min.js"></script>

  <div id="container"> 
    <div class="row">
      <div class="col nine" style="height: 20px">FISHERY ACTIVITY</div>
      <div class="col one" style="height: 20px"><a href="prismic/index.html">BLOG</a></div>
      <div class="col one" style="height: 20px"><a href="https://github.com/shangyanyan/DataVisualizationTutorial">TUTORIAL</a></div>
    </div>

    <div class="row">
      <div class="col twelve" style="height: 800px"> 
        <img src="Hero.jpg" alt="Hero Image" title="Hero Image" style="width: 100%; height: 800px" />
      </div>
      <div class="col twelve"></div>
    </div>

    <div class="row">
      <div class="col twelve" ><a href="http://shangyanyan.wixsite.com/graduateportfolio">MORE ABOUT YANYAN</a></div>
    </div>


  </div>
</body>
</html>
```

**Setup [Prismic.io](https://prismic.io/) for Content Management**
- [Introducing prismic.io](https://github.com/zachpino/id_proto/tree/master/week5)

### MY CODES
```
        <!DOCTYPE html>
	<html>
	<head>
		<title>Blog</title>

		
		<meta name="prismic-api" content="https://fishery-and-ocean-health.prismic.io/api">
		
		<script src="js/ejs.js"></script>
		<script src="js/prismic.min.js"></script>
		<script src="js/prismic.singlepage.js"></script>

	</head>
	<body>

		<script type="text/prismic-query" data-binding="posts">

			[
			[:d = at(document.type, "post")]
			]

		</script>

		<h1>[%= posts.length %] Posts are Available!</h1>

		[% posts.forEach(function(post) { %]

		<div>
			<h2>[%= post.getText('post.title') %]</h2>
			<img data-src="[%= post.getImageView('post.hero', 'main').url %]" width="100%">
			<p>
				[%= post.getText("post.body") %]
			</p>
			<hr />
		</div>
		[% }) %]

	</body>
	</html>
	```

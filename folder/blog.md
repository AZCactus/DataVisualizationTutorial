## Upload Blog

**Basic of HTML and CSS**
- [Standing Up a Server](https://github.com/zachpino/id_proto/tree/master/week1)
- [HTML Elements and the Box Model](https://github.com/zachpino/id_proto/tree/master/week2)
- [CSS Positioning](https://github.com/zachpino/id_proto/tree/master/week3)
- [Responsive Design](https://github.com/zachpino/id_proto/tree/master/week4)

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

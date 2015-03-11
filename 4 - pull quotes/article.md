A pull quote is a highlighted section of text that stands out in an article or blog post. On sites with a lot of reading content, particularly news sites, pull quotes are useful to highlight important parts of the article. They tend to sit inside a coloured box, and the font size is bigger. In this lab, we use a snippet of neat JQuery to add pull quotes to any section of text wrapped in a certain div class.

## Why Use A Script?

You’re probably thinking, “hey, I can just add the extra classes permanently in my HTML and voila, easy!” Well, that may be true, but that means re-writing HTML every single time. If you’re on a blogging software like WordPress (and you probably are blogging a lot if you have the need for pull quotes), this is even more of a hassle. Instead, you can just type out your full article, then scan it over and add a simple class to any important sections of text. Without further ado, let’s get into the coding and markup.

## The Process

The process is so simple, really. We just add a class of .pq to any important sections of text that we want to highlight. Then, we target those classes with JQuery, clone them, and add a new class to that section. Then, it’s up to our CSS to style the section of text. In the HTML, highlighted sections are wrapped in a span like this.

```language-markup
<span class="pq">Eggplant grape cauliflower eggplant potato gram chickweed groundnut aubergine bok choy celery garlic artichoke.</span>
```

These spans don’t follow any particular style in CSS, because we want them to flow with the regular content. Giving these spans a class though, we can use jQuery to target them, clone them, remove the current class, and add a new class, in that order. Here’s what the JQuery looks like. Of course, make sure you have the latest version of JQuery included.

```language-javascript
$(document).ready(function() {
    $('span.pq').each(function() {
        var quote = $(this).clone();
        quote.removeClass('pq');
        quote.addClass('pullquote');
        $(this).before(quote);
    }); // end each
}); // end ready
```

This clones all sections that have a class `.pq` attached to it, removes the `.pq` class, adds a new class `.pullquote`, and outputs the newly classed section of text. Now, it’s up to a little CSS to pull those quotes off to the side. Here’s what our CSS might look like.

```language-css
.pullquote {
	float: right;
	clear:right;

	background-color:rgb(100,150,200);
	border: 1px solid rgb(50,100,150);

	width: 300px;
	margin:8px 0 8px 8px;
	padding: 4px;

	color: rgb(255,255,255);
	text-shadow:-1px 1px rgb(50,100,150);
	font-style: italic;	
}
```

And voila, we are done. Pretty easy and neat, huh? Style this pull quote section as you please to match your site, and you’re good to go! I hope you enjoyed it, and please leave a comment below if you did.
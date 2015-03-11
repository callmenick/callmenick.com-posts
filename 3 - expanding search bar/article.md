<p class="text-align--center">
<a href="http://callmenick.com/lab-demos/3-expanding-search-bar/expanding-search-bar-source.zip" class="button button--inline-block button--medium">Get Source</a>
<a href="http://callmenick.com/lab-demos/3-expanding-search-bar/" class="button button--inline-block button--medium">View Demo</a>
</p>

A normal search bar is boring nowadays. Our search bar will neatly fit into a navigation menu with a neat icon label. When the icon label is clicked, the search bar will transition into view using CSS transitions. Our expanding search bar with CSS transitions looks like this unexpanded:

![Search bar closed](/files/2015-02/search_1.png)

and this expanded:

![Search bar closed](/files/2015-02/search_2.png)

Let’s take a look at the HTML, CSS, and JavaScript. As always, I’ll be using the box-sizing:border-box property on all my elements. I’ve also included the classie.js micro-library for class helper functions.

## The HTML

```html
<div id="navigation-bar" class="clearfix">    
    <form id="search" action="#" method="post">
        <div id="label"><label for="search-terms" id="search-label">search</label></div>
        <div id="input"><input type="text" name="search-terms" id="search-terms" placeholder="Enter search terms..."></div>
    </form>
    <nav>
        <ul>
            <li><a href="#">Nav 1</a></li>
            <li><a href="#">Nav 2</a></li>
            <li><a href="#">Nav 3</a></li>
            <li><a href="#">Nav 4</a></li>
        </ul>
    </nav>
</div>
```

The HTML is standard, with a navigation bar and two floated elements in it. One is the form, and the other is the navigation itself. Now, let’s look at the CSS, which is where the first set of magic happens.

## The CSS

```css
#navigation-bar {
    position: relative;
    height: 60px;
}
#search {
    position: absolute;
    top: 0;
    left: 0;
    width: 60px;
    height: 60px;
}
#label {
    width: 60px;
    height: 60px;
    position: relative;
    z-index: 20;
}
#label label {
    display: block;
    width: 60px;
    height: 60px;
    background: url("../img/search.png") 0 0;
    font-size: 0;
    color: rgba(0, 0, 0, 0);
    text-indent: -9999px;
    cursor: pointer;
}
#label label:hover {
    background: url("../img/search.png") -60px 0
}
#label.active label {
    background: url("../img/search.png") -60px 0
}
#input {
    position: absolute;
    top: 0;
    left: 60px;
    width: 450px;
    height: 60px;
    z-index: 5;
    overflow: hidden;
}
#input input {
    display: block;
    position: absolute;
    top: 0;
    left: -450px;
    width: 450px;
    height: 100%;
    margin: 0;
    padding: 0 10px;
    border: none;
    background-color: #23688b;
    color: #fff;
    font-size: 18px;
    backface-visibility: none;
    border-radius: 0;
    transition: left 0;
}
#input input:focus {
    outline: none
}
#input.focus {
    z-index: 20
}
#input.focus input {
    left: 0;
    transition: left 0.3s;
}
nav {
    padding-left: 70px;
    position: relative;
    z-index: 10;
}
nav ul {
    list-style-type: none;
    margin: 0;
    padding: 0;
}
nav li {
    display: inline-block;
    height: 60px;
    line-height: 60px;
    font-size: 24px;
    font-family: "Oswald", sans-serif;
    font-weight: 300;
    text-transform: uppercase;
}
nav a {
    display: block;
    padding: 0 15px;
    color: #c8c8c8;
}
nav a:hover {
    color: #3296c8
}
```

Our CSS is a bit fussy looking at first, but broken down it’s actually quite simple. We have a search form that’s given a fixed width and height, which is the width of our label icon (the search icon). We also have a navigation bar that’s given a fixed height also. Both these elements are floated left in the parent div, the #navigation-bar div. Inside the search form, the label is displayed as a block element with a z-index above the rest. The search input div container is given a width of 450px, and an absolute position, so it lies above the nav. When the search div has a class of focus, the actual search input transitions into view. The z-index of the search div gets raised higher than the z-index of the nav here, so that it displays above it.

Some important things to know here are that I removed the default outline property when inputs are focused. I also removed all default styling, and made the input a block type element with a solid background colour. The label stays active as long as the input is focused, and while the label is active, the icon changes. I used an image sprite for this, which is in the source code. The navigation is fairly standard, with some inline block list elements inside a parent nav tag. Now let’s take a look at the JavaScript that invokes all the class adding/removing.

## The JavaScript

```javascript
(function(window){

	// get vars
	var searchEl = document.querySelector("#input");
	var labelEl = document.querySelector("#label");

	// register clicks and toggle classes
	labelEl.addEventListener("click",function(){
		if (classie.has(searchEl,"focus")) {
			classie.remove(searchEl,"focus");
			classie.remove(labelEl,"active");
		} else {
			classie.add(searchEl,"focus");
			classie.add(labelEl,"active");
		}
	});

	// register clicks outisde search box, and toggle correct classes
	document.addEventListener("click",function(e){
		var clickedID = e.target.id;
		if (clickedID != "search-terms" && clickedID != "search-label") {
			if (classie.has(searchEl,"focus")) {
				classie.remove(searchEl,"focus");
				classie.remove(labelEl,"active");
			}
		}
	});
}(window));
```

The JavaScript listens for a "click" on the label. If it is clicked, a couple checks are made:

1. If the text input has the focus class, we remove the focus class from it. We also remove the active class from the label
2. If the text input does NOT have the focus class, we add it and also add the active class to the label

Another piece of neat functionality is listening for clicks on the entire document. If the click registered is outside the search input and label, we remove the focus and active classes from the input and label, and the input goes back out of view. And that’s it!

Feel free to view the demo or download the source by clicking the links below. Also, please leave any comments, suggestions, and feedback! Thanks for reading.

<p class="text-align--center">
<a href="http://callmenick.com/lab-demos/3-expanding-search-bar/expanding-search-bar-source.zip" class="button button--inline-block button--medium">Get Source</a>
<a href="http://callmenick.com/lab-demos/3-expanding-search-bar/" class="button button--inline-block button--medium">View Demo</a>
</p>
<p class="text-align--center">
<a href="http://callmenick.com/tutorial-demos/tile-navigation/tile-navigation-source.zip" class="button button--inline-block button--medium">Get Source</a>
<a href="http://callmenick.com/tutorial-demos/tile-navigation/" class="button button--inline-block button--medium">View Demo</a>
</p>

## Introduction

Full width tile navigations are a crisp way to display your site’s navigation and give end to end coverage, creating a very full-bodied effect for the user. In this tutorial, we’re going to make it fully responsive, and throw in some quick JavaScript at the end to show/hide the navigation on smaller screen widths via a navigation toggle button. We’ll toss in some CSS3 transitions too for a little extra punch. Let’s dig in!

## Structuring The Menu

Our navigation menu is nothing we haven’t seen before…some links inside an unordered list wrapped up inside a `nav` tag. I have a navigation toggle button as well that will display on smaller screens, allowing the users to show/hide the navigation at will and save on screen real estate. Here’s a look at the HTML:

```html
<a href="#" class="nav-toggle">Toggle Navigation</a>
<nav class="cmn-tile-nav">
  <ul class="clearfix">
    <li class="colour-1"><a href="#">HTML</a></li>
    <li class="colour-2"><a href="#">CSS</a></li>
    <li class="colour-3"><a href="#">Sass</a></li>
    <li class="colour-4"><a href="#">JS</a></li>
    <li class="colour-5"><a href="#">PHP</a></li>
    <li class="colour-6"><a href="#">Ruby</a></li>
    <li class="colour-7"><a href="#">iOS</a></li>
    <li class="colour-8"><a href="#">Android</a></li>
  </ul>
</nav>
```

Now let’s dig into our starting CSS. Because I’m taking on a mobile-first approach, our navigation is initially hidden. When a class of `.open` is added to it, it will display. We reset the unordered list and let our list elements display as block elements. The only transition they will have is for the background colour. Our actual link elements will also be block elements to fill the space nicely, and they will have transitions on the `background` and `transform` properties. To start, when we hover over link elements, we want them to nudge to the right smoothly. This is achieved by applying the `transform` property on `:hover` on the `a` element. We don’t want it to cause horizontal scrollbars or unexpected layout breaks, so our `li`'s have the `overflow` `property` set to hidden. We give each `li` and `a` element the same set of `background` properties in normal and hover states, so repeat that for all 8 tiles (I only showed it for one below). Here’s the CSS:

```css
/* nav styles */
nav.cmn-tile-nav {
  display: none;
}
nav.cmn-tile-nav.open {
  display: block;
}
nav.cmn-tile-nav ul {
  list-style: none;
}
nav.cmn-tile-nav li {
  display: block;
  overflow: hidden;
  transition: background 0.3s;
}
nav.cmn-tile-nav a {
  display: block;
  padding: 20px;
  color: #fff;
  transition: background 0.3s, transform 0.3s;
}
nav.cmn-tile-nav a:hover {
  transform: translateX(20px);
}

/* repeat this section for the 8 different colours */
/* --- BEGIN --- */
nav.cmn-tile-nav li.colour-1,
nav.cmn-tile-nav li.colour-1 a {
  background-color: #28aadc;
}
nav.cmn-tile-nav li.colour-1:hover,
nav.cmn-tile-nav li.colour-1:hover a {
  background-color: #166888;
}
/* --- END --- */

/* smoother transitions */
nav.cmn-tile-nav li,
nav.cmn-tile-nav a {
  transform: translate3d(0, 0, 0);
}
```

## The Media Queries

Now, let’s implement some media queries to our design. For our first breakpoint at 480px, we’ll automatically display the navigation instead of having it hidden initially. Our tiles will each be 50% width and floated to the left, and that’s achieved by the following CSS:

```css
@media all and (min-width: 480px) {
  nav.cmn-tile-nav {
    display: block;
  }
  nav.cmn-tile-nav li {
    width: 50%;
    float: left;
  }
}
```

For our second breakpoint at 768px, our tiles will be 25% width, displaying 2 rows of 4. We’ll align our text to the center and shift up the padding a little bit. This time though, instead of translating the text to the left on hover, we’ll translate it upwards a bit. Here’s the CSS:

```css
@media all and (min-width: 768px) {
  nav.cmn-tile-nav li {
    width: 25%;
  }
  nav.cmn-tile-nav a {
    text-align: center;
    padding: 60px 20px 20px 20px;
  }
  nav.cmn-tile-nav a:hover {
    transform: translateX(0);
    transform: translateY(-20px);
  }
}
```

For our third and final breakpoint at 1024px, we’ll change it up a little bit. The tiles will be displayed in one row. That’s 8 tiles in 1 row – a width of 12.5% each. We’ll reset the overflow property on our li elements though, and nudge the link down a bit on hover. It will give the illusion that the box is getting taller on hover without interrupting the flow directly below. Here’s the CSS:

```css
@media all and (min-width: 1024px) {
  nav.cmn-tile-nav li {
    overflow: visible;
    width: 12.5%;
  }
  nav.cmn-tile-nav a {
    padding: 80px 20px 20px 20px;
  }
  nav.cmn-tile-nav a:hover {
    transform: translateY(20px);
  }
}
```

## The Navigation Toggle Button

Our navigation toggle button takes on some simple styling to match the tiles. At the first media query breakpoint though (480px), we hide it. This is because the navigation is automatically displayed at this breakpoint. Here’s the CSS:

```css
a.nav-toggle {
  display: block;
  margin-bottom: 20px;
  padding: 20px;
  background-color: #dce6f0;
  color: #646464;
  text-align: center;
}
a.nav-toggle:hover {
  background-color: #c8d2dc;
}
/* the breakpoint same as above */
@media all and (min-width: 480px) {
  a.nav-toggle {
    display: none;
  }
}
```

## Some Basic JavaScript

I also implemented some basic javascript required to get the navigation toggle to work. I’m using classie.js to add and remove classes easily. Here it is:

```javascript
(function(window){

  var nav = document.querySelector("nav.cmn-tile-nav"),
      nav_toggle = document.querySelector("a.nav-toggle");

  nav_toggle.addEventListener("click", function(e){
    e.preventDefault();
    classie.toggle(nav, "open");
  });

})(window);
```

## Wrap Up

With the current host of CSS techniques at our disposal, we’re able to easily achieve layouts like this. We’re also able to add a little action to it with some implementation of CSS3 transitions. Don’t stop there though…try making the tiles even more attractive. Add some icons, some borders maybe, and play around with different animation techniques. There are lots of possibilities! I hope you enjoyed this tutorial, and don’t forget you can download the source and view the demo by clicking the links below! If you have any questions, comments, or feedback, feel free to leave them below. Thanks for stopping by.

<p class="text-align--center">
<a href="http://callmenick.com/tutorial-demos/tile-navigation/tile-navigation-source.zip" class="button button--inline-block button--medium">Get Source</a>
<a href="http://callmenick.com/tutorial-demos/tile-navigation/" class="button button--inline-block button--medium">View Demo</a>
</p>
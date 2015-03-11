## The Reasoning

With the mobile and responsive era upon us, it’s important that our projects are streamlined for expansion and scalability. The long-standing aesthetic of triangular images to indicate drop downs, leader text, navigation hints, and such, still exists strongly. With some simple CSS trickery though, we can eliminate the use of images, and create pure CSS triangles. Above is a graphic of the the triangles we’re going to create. Yes, I started my numbering at zero.

## Digging in

For the purposes of this demonstration, each of our triangles will reside in its own div. This breaks the idea of semantic markup though, so in practice, you’re probably better off applying them to pseudo selectors `::before` or `::after`. [See this tutorial for some info about these (and other) selectors](http://callmenick.com/2014/04/13/under-the-radar-css-selectors/). It’s important to note that the `box-sizing: border-box` property should be applied across the entire document. [It’ll make your life a whole lot easier.](http://callmenick.com/2014/03/26/box-sizing-reset-border-box/) Here’s what the markup will look like:

```html
<div class="triangle triangle-0"></div>
<div class="triangle triangle-1"></div>
<div class="triangle triangle-2"></div>
<div class="triangle triangle-3"></div>
<div class="triangle triangle-4"></div>
<div class="triangle triangle-5"></div>
```

And here’s some common CSS to get us going:

```css
*,
*:before,
*:after {
  box-sizing: border-box;
}
body,
html {
  width: 100%;
  height: 100%;
}
body {
  padding: 40px;
  background-color: rgb(255,80,100);
  text-align: center;
}
.triangle {
  display: inline-block;
  margin: 0 20px;
  vertical-align: middle;
}
```

We’re going to use a combination of solid and transparent borders to show and hide certain sides of the div area. We’re making use of the fact that CSS borders meet each other at an angle, so transparent borders will give the diagonal effect on the touching solid side. A little math is necessary, but it’s pretty basic. Alright, let’s go.

## Triangle #0

Triangle \#0 points south-east. Here’s the CSS:

```css
.triangle-0 {
  width: 100px;
  height: 100px;
  border-bottom: solid 50px rgb(200,30,50);
  border-right: solid 50px rgb(200,30,50);
  border-left: solid 50px transparent;
  border-top: solid 50px transparent;
}

## Triangle #1

Triangle number one is like #0, but points south-west instead. Here’s the CSS:

```css
.triangle-1 {
  width: 100px;
  height: 100px;
  border-bottom: solid 50px rgb(200,30,50);
  border-left: solid 50px rgb(200,30,50);
  border-right: solid 50px transparent;
  border-top: solid 50px transparent;
}
```

## Triangle #2

Triangle #2 points directly west. Here’s the CSS:

```css
.triangle-2 {
  width: 50px;
  height: 100px;
  border-right: solid 50px rgb(200,30,50);
  border-bottom: solid 50px transparent;
  border-top: solid 50px transparent;
}
```

## Triangle #3

Triangle #3 is the exact opposite of #2, pointing directly east. Here’s the CSS:

```css
.triangle-3 {
  width: 50px;
  height: 100px;
  border-left: solid 50px rgb(200,30,50);
  border-bottom: solid 50px transparent;
  border-top: solid 50px transparent;
}
```

## Triangle #4

Triangle #4 points directly north. Here’s the CSS:

```css
.triangle-4 {
  width: 100px;
  height: 50px;
  border-bottom: solid 50px rgb(200,30,50);
  border-left: solid 50px transparent;
  border-right: solid 50px transparent;
}
```

## Triangle #5

Triangle #5 is exactly opposite to #4, and points directly south. Here’s the CSS:

```css
.triangle-5 {
  width: 100px;
  height: 50px;
  border-top: solid 50px rgb(200,30,50);
  border-left: solid 50px transparent;
  border-right: solid 50px transparent;
}
```

## Code Pen Demo

You can see them all in action by viewing the Pen below.

<p data-height="257" data-theme-id="5513" data-slug-hash="cBkvE" data-default-tab="result" class='codepen'>See the Pen <a href='http://codepen.io/callmenick/pen/cBkvE/'>CSS Triangle</a> by Nick Salloum (<a href='http://codepen.io/callmenick'>@callmenick</a>) on <a href='http://codepen.io'>CodePen</a>.</p>
<script async src="//codepen.io/assets/embed/ei.js"></script>

## Wrap Up

And there you have it, CSS triangles in any direction! They are all easily scalable, expandable, and editable. I hope this snippet was useful for you. Feel free to leave any comments or feedback below, and thanks for stopping by.
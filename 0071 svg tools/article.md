SVG, or "scalable vector graphics", is an XML markup language used to describe 2D graphics. It's a W3C recommendation, and is designed to work with other W3C standards like CSS and DOM. It has made great strides on the web in recent years, and is being widely incorporated in a range of project types. Here's a list of some of some tools that can greatly increase your workflow when it comes to SVG and the web. Note that there are many more out there, but these are just some of my favourites.

## 1) Vector Software

Any vector editing and creating software ultimately is an SVG tool, and there are a few to choose from. Let's take a look at 3 of the top tools. 

### 1a) Adobe Illustrator

Leading the pack with no surprise is [Adobe Illustrator](http://www.adobe.com/products/illustrator.html). Adobe Illustrator is an extremely powerful vector graphics software, used across many industries far and wide. With the rise of SVG on the web, it's utility as an SVG tool has greatly increased. It does, however, come with a price. If graphics is your thing though, and you're use of SVG is substantial, it might be worth investing a few pennies!

### 1b) Corel Draw

[Corel Draw](http://www.coreldraw.com/rw/product/graphic-design-software/), a member of the Corel family of design products, is another highly capable tool. Corel has long been a rival to Adobe in the graphics world, but with Adobe's dominant rise in many other software areas (including web), Corel seems to be on the back burner a bit. Many graphic artists around the world still swear by it however, but if your interests are strictly in the realm of SVG, it might not be your go-to software. It does after all come with its own price tag.

### 1c) SVG-edit

The list of vector software would of course be incomplete without a good old open source tool, and where else to find it but in Google's code drive. [SVG-edit](https://code.google.com/p/svg-edit/) is an open source, fast, web-based SVG editor. It is JavaScript based, and works in any modern browser. Coming in at a whopping $free.99, it's the perfect solution for developers who want to visually create graphics, and output the SVG code.

## 2) SVG Libraries For Scripting SVG

On the visual side of things, SVG is a collection of shapes, strokes, colours, and the works, grouped and output visually on screen. However, any SVG file can actually be opened in a text editor, and that's where the inner workings of SVG can be seen. In my tutorial on [getting started with inline SVG](http://callmenick.com/2014/10/19/getting-started-inline-svg/), I go into detail on scripting SVG directly in your document to produce an output on screen. However, there are some libraries out there that streamline SVG scripting, and make for easier interactivity. Let's take a look at two of them.

### 2a) SnapSVG

[SnapSVG](http://snapsvg.io/) is my favourite library for working with SVG recently, and it's very easy to get up and running with it in no time. According to them, it's an excellent way to create interactive, resolution independent vector graphics that will look good on any screen size. I couldn't agree more with that statement. The library is well documented, boasts a huge range of features and methods, and ships with some demos for your perusal. Their [getting started guide](http://snapsvg.io/start/) is easy to follow, and will get your feet wet and your ideas flowing. Creating a circle, for example, is as easy as this:

```javascript
var s = Snap("#svg");
var circle = s.circle(150, 150, 100);
circle.attr({
    fill: "#bada55",
    stroke: "#000",
    strokeWidth: 2
});
```

The output will be a circle of radius 100px, with the x and y coordinates at 150px from the SVG origin. Setting the attributes is really simple too, as seen above. Animating the circle is a breeze too:

```javascript
circle.animate({
  r: 50
}, 1000);
```

This will animate the radius of the circle linearly to 50px over the course of 1 second. Did I mention that Snap ships with a host of easing functions?

### 2b) RaphaelJS

RaphaelJS, like SnapSVG, is a JavaScript library with the solitary task of making your SVG workflow a breeze. It's also highly documented on their site, and there are a bunch of demos from which you can peep the source code and get some ideas of the possibilities and inner workings. It's instantiation and usage is also very easy to grasp. To create the same circle as above, we'd do this:

```javascript
var paper = Raphael("#svg");
var paper = Raphael("svg");
var circle = paper.circle(150, 150, 100);
circle.attr({
  fill: "#bada55",
  stroke: "#000",
  "stroke-width": "2"
});
```

As you can see, it's a pretty similar syntax to Snap SVG! As you may expect, animating the circle is just as easy:

```javascript
circle.animate({
  r: 50
}, 1000);
```

Voila. An just like SnapSVG, Raphael also ships with its own set of easing functions. I suppose it's just a matter of preference when you're deciding your library of choice, but I'd say just pick one and go with it.

## 3) SVG Optimisation Tools

If you're creating your SVG's up front in any graphics software of your choice, and then wanting to drop them into your document before manipulation or output, it's highly recommended that you optimise your file first. Most graphics softwares output SVG with a bunch of unnecessary hoo-ha's that add weight to your page and overall excess baggage. Here's a couple tools to get you started.

### 3a) Command Line SVG Optimiser (SVGO)

[SVGO](https://github.com/svg/svgo) is a NodeJS-based tool that runs via the command line, packing in a huge number of functions. It can clean up attributes, remove comments, remove meta data, convert colours from one string format to another, and it can even minify your SVG code. It's my tool of choice, and usage is as simple as calling `svgo` on your file like this:

```
svgo foo.svg
```

### 3b) Web-Based SVG Optimiser (by Peter Collingridge)

If NodeJS and the command line aren't your thing, you should definitely check out Peter Collingridge's web based [SVG optimiser tool](http://petercollingridge.appspot.com/svg_optimiser). It doesn't pack the full range of features as SVGO, but is beyond suitable for simpler files.

## Wrap Up

What we've seen in this article is just a touch on the surface of SVG tools available for us. I'm a long-standing user of Adobe Illustrator, so it's my tool of choice. Snap SVG is also my preferred library for scripting SVG, and SVGO is unbeatable in my opinion. But that doesn't make them all the pick of the bunch. It all comes down to preference, so try them all and set up a workflow that suits you!

## Worthy Mentions

Here are some other worthy mentions that I know about but didn't write about in this article due to lack of depth of knowledge. Try them out for yourself!

* https://inkscape.org/en/ - a free and open source professional vector graphics editor for Windows, Mac OS X and Linux.
* http://svgjs.com/ - A lightweight library for manipulating and animating SVG.
* http://jsxgraph.uni-bayreuth.de/wp/ - JSXGraph is a cross-browser library for interactive geometry, function plotting, charting, and data visualization in a web browser.

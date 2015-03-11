## What Is SVG?

SVG stands for "scalable vector graphics", and the name itself is very suggestive. SVG gives us the power to scale a graphic from 100% to 1000% without losing any quality at all. An SVG file is all XML, and is a declaration of various shapes and paths inside an SVG object. Each of these shapes/paths consists of a collection of points, positions, lengths, radius, etc., depending on the shape type. If you've ever worked in any vector graphics software (like Adobe Illustrator or Corel Draw), you've created vector graphics that can be easily outputted as SVG.

It's been particularly useful and commonplace for things like icons, because we can use one small SVG file and scale it to bigger sizes without any loss. SVG support and power has been growing quite rapidly in the web though, and libraries like Snap.svg have given us the ability to create some amazing stuff. Today though, I'm going to get into some basics, and show you some really cool things that can be achieved with SVG.

## Using Inline SVG

There are three main implementations of SVG:

1. SVG as a background image
2. SVG as a `src` attribute
3. Inline SVG

The first two are basic and very common implementations of SVG. You have your SVG file, and you link to it like you would to any other media file. The third - inline SVG - will be the focus of this tutorial. Inline SVG spits out all of our XML data directly in our document, and allows us to access the various elements via CSS and JavaScript. This gives us great control over manipulation of things like fill, stroke, position, opacity, and a host of other properties and attributes. Let's take a look at how we declare a new inline SVG element, and the creation of a few basic shapes.

## Declaring An SVG Element

Because SVG is an XML markup language, we need to declare the namespace for our SVG element. You can take a look at the MDN page about namespaces [here](https://developer.mozilla.org/en/docs/Web/SVG/Namespaces_Crash_Course), but in a nutshell, we're looking at something like this:

```html
<svg xmlns="http://www.w3.org/2000/svg">
  <!--  svg stuff here  -->
</svg>
```

There's a host of other attributes that we can declare, but for now, we're going to look at these three:

1. Width - this represents the horizontal length in the user coordinate system.
2. Height - this represents the vertical length in the user coordinate system.
3. View box - this allows us to specify that a given set of graphics stretch to fit a particular container element. The value of the `viewBox` attribute is a list of four numbers `min-x`, `min-y`, `width` and `height`.

Now, our SVG element might look something like this:

```html
<svg xmlns="http://www.w3.org/2000/svg" width="400" height="300" viewBox="0 0 400 300">
<!--  svg stuff here  -->
</svg>
```

For a full list of attributes, check the [attribute reference](https://developer.mozilla.org/en-US/docs/Web/SVG/Attribute) on MDN. Let's create some shapes now!

## Creating SVG Shapes

Now that we've declared our SVG with some useful attributes, it's time to create some shapes. There's a host of elements that we can now include inside our `svg` tag, but we're going to look at four common elements in this tutorial:

1. `circle` - a basic shape, used to create circles based on a centre point and radius.
2. `rect` - a basic shape, used to create rectangles based on the position of a corner, width, and height.
3. `polygon` - a basic shape, defines a closed shape based on coordinate pairs connected by straight line segments.
4. `path` - a generic element that can be used to define any shape. All basic shapes can be made with paths.

### The Circle Element

With the `circle` element, we have 3 specific attributes. They are `cx`, `cy,` and `r`. `cx` and `cy` are the x and y-axis coordinates of the centre of the element, and they default to zero if they aren't specified. `r` is the radius of the circle. We may then have some markup looking like this:

```html
<svg xmlns="http://www.w3.org/2000/svg" width="100" height="100" viewBox="0 0 100 100">
  <circle cx="50" cy="50" r="50"></circle>
</svg>
```

And our output like this:

<svg xmlns="http://www.w3.org/2000/svg" width="100" height="100" viewBox="0 0 100 100"><circle cx="50" cy="50" r="50"></circle></svg>

### The Rect Element

Using the `rect` element is just as intuitive as the `circle` element. In it's most basic form, we specify the `x` and `y` position of the top left corner, and specify the `width` and `height`. Our markup may look like this:

```html
<svg xmlns="http://www.w3.org/2000/svg" width="200" height="100" viewBox="0 0 200 100">
  <rect x="0" y="0" width="200" height="100"></rect>
</svg>
```

And the output like this:

<svg xmlns="http://www.w3.org/2000/svg" width="200" height="100" viewBox="0 0 200 100"><rect x="0" y="0" width="200" height="100"></rect></svg>

### The Polygon Element

We define our `polygon` element by a collection of x-y coordinates listed inside the `points` attribute. These coordinates are connected in order via straight line segments. We may have some markup looking like this:

```html
<svg xmlns="http://www.w3.org/2000/svg" width="200" height="200" viewBox="0 0 200 100">
  <polygon points="0,50 50,0 150,0 200,50 150,100 50,100"></polygon>
</svg>
```

And our output like this:

<svg xmlns="http://www.w3.org/2000/svg" width="200" height="100" viewBox="0 0 200 100"><polygon points="0,50 50,0 150,0 200,50 150,100 50,100"></polygon></svg>

### The Path Element

The `path` element is the most powerful element at our disposal as far as shapes go. Inside the `path` element, we can define our `d` attribute, which is the definition by which the path follows. The `d` attribute let's us specify certain movements by using a series of commands. We can use line commands and curve commands to construct complex shapes. In reality, you'll probably not want to write out cubic Bezier's by hand, but create them in a vector graphics program and export the SVG. But it's still good to know what's going on.

Let's create a simple square using some line commands. At our disposal, we have:

* M x y - this moves us to an x-y coordinate
* L x y - this draws a line to an x-y coordinate from our current position
* H x - this draws a horizontal line to an x coordinate
* V y - this draws a vertical line to a y coordinate
* Z - this closes the path.

With that in mind, here's some sample markup for a simple square:

```html
<svg xmlns="http://www.w3.org/2000/svg" width="200" height="200" viewBox="0 0 200 200">
  <path d="M 0 0 H 200 V 200 H 0 Z"></path>
</svg>
```

We started at (0 0), moved horizontally to (200 0), moved vertically to (200 200), moved horizontally to (0 200), and closed the path. Our output looks like this:

<svg xmlns="http://www.w3.org/2000/svg" width="200" height="200" viewBox="0 0 200 200"><path d="M 0 0 H 200 V 200 H 0 Z"></path></svg>

Creating curves is a slightly more complicated procedure. But at our disposal, we have cubic Bezier's, quadratic curves, and arcs. As an example, let's look at the building of a cubic Bezier.

At this point, if you want to learn more about Bezier curves and how they are constructed, I suggest you [read this reference](http://en.wikipedia.org/wiki/B%C3%A9zier_curve). But as a quick summary, a cubic Bezier curve is defined by a start point, an end point, an two coordinate sets to which we draw tangential lines from the start and end points (yay Mechanical Engineering degree). In our construction of a cubic Bezier, we define our start point by moving to it using the "M" command, then initiate the cubic Bezier with the "C" command. Here's the markup for a simple curve:

```html
<svg xmlns="http://www.w3.org/2000/svg" width="200" height="100" viewBox="0 0 200 100">
  <path d="M 0 0 C 50 100, 150 100, 200 0"/>
</svg>
```

And here's the output:

<svg xmlns="http://www.w3.org/2000/svg" width="200" height="100" viewBox="0 0 200 100"><path d="M 0 0 C 50 100, 150 100, 200 0"/></svg>

## Adding Style To Our Elements

At this point, we've only touched the surface of what's possible. Let's go back to our circle example, and add some style. Remember that attribute reference I linked to earlier? Well there's a host of things there that we can control. Let's look at some common ones:

1. `fill` - the fill colour of the element
2. `fill-opacity` - the opacity of the fill colour
3. `stroke` - the stroke colour of the element
4. `stroke-width` - the width of the stroke
5. `stroke-opacity` - the opacity of the stroke colour

Before anything, let's first add a class of `circle` to our circle so we can use it in our CSS. Now, let's style up our circle a bit. Here's the new and improved markup:

```html
<svg xmlns="http://www.w3.org/2000/svg" width="200" height="200" viewBox="0 0 200 200">
  <circle class="circle" cx="100" cy="100" r="90" fill="#3399cc" stroke="#333333" stroke-width="5"></circle>
</svg>
```

Now, let's add some transitions and create some hover effects using CSS. We'll start to see some of the real beauty of inline SVG here, as we are going to manipulate particular attributes. Here's some CSS:

```css
.circle {
  transition: stroke 0.3s, fill 0.3s;
}

.circle:hover {
  stroke: #3399cc;
  fill: #333333;
}
```

And here's the output:

<style>.circle { -webkit-transition: stroke 0.3s, fill 0.3s; transition: stroke 0.3s, fill 0.3s; } .circle:hover { stroke: #3399cc; fill: #333333;}</style>
<svg xmlns="http://www.w3.org/2000/svg" width="200" height="200" viewBox="0 0 200 200"><circle class="circle" cx="100" cy="100" r="90" fill="#3399cc" stroke="#333333" stroke-width="5"></circle></svg>

Pretty awesome! Again, I encourage you to take a look at the [attribute reference](https://developer.mozilla.org/en-US/docs/Web/SVG/Attribute) and the [element reference](https://developer.mozilla.org/en-US/docs/Web/SVG/Element) for a full list of what's available.

## What Next?

Creating a circle with a transitioning fill and stroke probably has a 0.001% chance of making it into your production app/site. But the knowledge gained by mastering the basic implementations of SVG can take you far. You'll most likely want to create your SVG's in a vector program (or download them from the net), and access various paths and shapes by way of classnames, CSS, and JavaScript. Speaking of JavaScript, SVG is another DOM element, and so are all of its sub elements. That means you can access and play with all elements and attributes via JavaScript, giving you some really neat control over manipulation possibilities.

## Saving SVG For Web

Industry leading vector software like Adobe Illustrator allows you to save files as .svg. You can reopen these in graphics software, or you can open it in a text editor. When you do that, you'l notice a nice output of all those SVG paths and shapes that we spoke about in this tutorial. But this isn't quite ready for web just yet. You can clean it up manually (boo hoo), or use either of these two tools:

1. The online [SVG Optimiser](http://petercollingridge.appspot.com/svg_optimiser) by Peter Collingridge. Good cause it's all online.
2. This awesome node tool called [SVGO](https://github.com/svg/svgo). It comes with a host of options, and runs easily right on your computer. It's my preferred tool, but it does require node.

## Wrap Up

This has been a highly theoretical tutorial, but it's main purpose was to get you thoroughly understanding the inner workings of SVG at a very fundamental level. By wrapping your head around some of the basics, you can progress and do some really awesome stuff. And you really should get on the SVG train, as support is high (IE9 and up), and possibilities are endless. A lot of people are doing some really interesting stuff with it already, and I'm hoping to get into some projects myself ASAP.

Thanks for reading, and if you have any questions, comments, or input, please provide it below and I'll get back to you as quickly as possible! Next time, some cool SVG stuff.
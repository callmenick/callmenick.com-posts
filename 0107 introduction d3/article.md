<p class="text-align--center">
<a href="#" class="button button--inline-block button--medium">Get Source</a>
<a href="#" class="button button--inline-block button--medium">View Demo</a>
</p>

## What Is D3JS?

[D3JS](https://d3js.org/) (just D3 for short) is commonly and mistakenly known as a charting library. While D3's powers do make dynamic charting on the web a joy, there is way more to it than that. D3 stands for **Data Driven Documents**, and is a powerhouse of a library that does exactly what its name suggests:

> D3.js is a JavaScript library for manipulating documents based on data. D3 helps you bring data to life using HTML, SVG, and CSS.

There are a host of reasons that you may want to use D3 for displaying, manipulating, and visualizing data:

1. Dynamic charts of all kinds (line, bar, pie, etc...)
2. System visualizations of all kinds (weather, population, etc...)
3. Building interactive maps

The list can keep going on, but before your imagination runs wild, let's dive into some basics.

## Getting Started

D3 is a JavaScript library, but other than JavaScript itself, it has no hard requirements. Using a DOM library is optional, but in this tutorial, we'll stick with the DOM API. To get started right away, just include a link to the CDN hosted library in your document:

```html
<script src="//d3js.org/d3.v3.min.js" charset="utf-8"></script>
```

There are many other ways to install D3 including [npm](https://www.npmjs.com/package/d3), [bower](http://bower.io/search/?q=d3), and others, but we'll just use the CDN for this tutorial. Alright, let's dive in!

## A Plunge into the Fundamentals

Behind every D3 powered visualization lives two main things:

1. A selection, which maintains a reference to a collection of elements from the DOM.
2. A data set, which is leveraged to bind bits of data to these elements contained in the selection.

With that in mind, there are two more important things to understand about D3 (which we'll cover in this tutorial):

1. There are a host of ways that we can operate on a D3 selector that include manipulating the contents of each element in the selection, adding transitions, changing their styles, altering content, removing the elements, and adding data.
2. At first, it may be easy to think that selections are a grouping of elements that already exist in the DOM. **That's not true.** In fact, because of the great variety of operations that exist on selections as well as the typically dynamic nature of data, selections can hold some sort of knowledge of elements that are yet to exist, or those that will cease to exist (depending on the associated data). D3 can pretty much see into the future! Wild.

D3's declarative approach also makes it very easy to chain together methods, thus minimizing verbosity in code whilst maximizing legibility and efficiency. Alright, it's time to conjure up some D3 magic.

## Basic Selections & Manipulations

There are four ways to make a selection in D3, and the following is taken from the [API docs](https://github.com/mbostock/d3/wiki/Selections#d3_select):

1. `d3.select(selector)` - Selects the first element that matches the specified selector string, returning a single-element selection.
2. `d3.select(node)` - Selects the specified node. This is useful if you already have a reference to a node.
3. `d3.selectAll(selector)` - Selects all elements that match the specified selector.
4. `d3.selectAll(nodes)` - Selects the specified array of elements. This is useful if you already have a reference to nodes.

We will learn more about all of these, but for now, let's begin with a very basic example using `d3.select(selector)`. Let's assume that we have an existing DOM element that we want to edit on the click of a button:

```html
<div id="selection" class="c-selection--1"></div>
<button id="toggle">Edit Me</button>
```

Let's add some very basic styling to our selection div as well:

```css
.c-selection--1 {
  width: 100px;
  height: 100px;
  background-color: #ccc;
}
```

Now, let's add a simple click event listener to our button that changes the div styles with D3:

```javascript
document.querySelector('#toggle').addEventListener('click', function() {
  d3.select('#selection')
    .style('width', '200px')
    .style('height', '200px')
    .style('background-color', 'rgb(40, 170, 220)');
});
```

## Entering & Exiting Data

## Wrap Up

And that’s a wrap! Don’t forget, you can view the demo and download the source by clicking the links below, and if you have and questions, comments, or feedback, you can also leave them below.

<p class="text-align--center">
<a href="#" class="button button--inline-block button--medium">Get Source</a>
<a href="#" class="button button--inline-block button--medium">View Demo</a>
</p>
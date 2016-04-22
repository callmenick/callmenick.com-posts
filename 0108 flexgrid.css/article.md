<p class="text-align--center">
<a href="https://github.com/callmenick/flexgrid.css" class="button button--inline-block button--medium">Get Source</a>
<a href="http://flexgrid.callmenick.com/" class="button button--inline-block button--medium">View Demo</a>
</p>

## Flexgrid.css

Flexgrid.css is a responsive, declarative grid system powered by flexbox.

## Installation

### As an `npm` module

You can include it in your project as an `npm` module by running the following:

```
npm install flexgrid.css --save
```

### Download Files Directly

You can grab the CSS files here:

* Uncompressed - [flexgrid.css](https://raw.githubusercontent.com/callmenick/flexgrid/gh-pages/css/flexgrid.css)
* Compressed - [flexgrid.min.css](https://raw.githubusercontent.com/callmenick/flexgrid/gh-pages/css/flexgrid.min.css)

## Usage

Grids can de used on generic HTML elements such as `div`, but resets are also in place to use them on `ul` and `ol`.

### Basic Usage

In the most basic form, you can create a grid where all cells occupy an equal amount of space:

```html
<div class="Grid">
  <div class="Grid-cell"></div>
  <div class="Grid-cell"></div>
  ...
</div>
```

If you want cells to occupy the amount of space that their content takes up, then use the `--auto` modifier:

```html
<div class="Grid">
  <div class="Grid-cell--auto"></div>
  <div class="Grid-cell--auto"></div>
  <div class="Grid-cell--auto"></div>
  <div class="Grid-cell--auto"></div>
  ...
</div>
```

### Responsive, Declarative Usage

In order to declaratively build a responsive grid in which cells occupy a certain percentage of width at a certain screen size, you can chain together size modifiers for the cells. When using this approach, the bare requirement is to declare the cell size at the smallest screen size, i.e. `xs`. The other sizes are optional. Here are all screen size modifiers:

* `xs`
* `sm`
* `md`
* `lg`
* `xl`
* `xx`

The grid is based on a 12 column layout, so when thinking about your display for one row, think that the span number must add up to 12. Here's a basic example:

```html
<ul class="Grid">
  <li class="Grid-cell--xs-2"></li>
  <li class="Grid-cell--xs-8"></li>
  <li class="Grid-cell--xs-2"></li>
</ul>
```

This grid will display cells the same at any screen size, in a `2:8:2` ratio (compared to the number of columns, 12). Let's make these grid cells change at various breakpoints:

```html
<div class="Grid">
  <div class="Grid-cell--xs-4 Grid-cell--md-3 Grid-cell--lg-2"></div>
  <div class="Grid-cell--xs-4 Grid-cell--md-3 Grid-cell--lg-2"></div>
  <div class="Grid-cell--xs-4 Grid-cell--md-3 Grid-cell--lg-2"></div>
  <div class="Grid-cell--xs-4 Grid-cell--md-3 Grid-cell--lg-2"></div>
  <div class="Grid-cell--xs-4 Grid-cell--md-3 Grid-cell--lg-2"></div>
  <div class="Grid-cell--xs-4 Grid-cell--md-3 Grid-cell--lg-2"></div>
  <div class="Grid-cell--xs-4 Grid-cell--md-3 Grid-cell--lg-2"></div>
  <div class="Grid-cell--xs-4 Grid-cell--md-3 Grid-cell--lg-2"></div>
  <div class="Grid-cell--xs-4 Grid-cell--md-3 Grid-cell--lg-2"></div>
  <div class="Grid-cell--xs-4 Grid-cell--md-3 Grid-cell--lg-2"></div>
  <div class="Grid-cell--xs-4 Grid-cell--md-3 Grid-cell--lg-2"></div>
  <div class="Grid-cell--xs-4 Grid-cell--md-3 Grid-cell--lg-2"></div>
</div>
```

In the above example, the cells will be in a ratio of `4:4:4` per row, making 4 rows in total, by default and at the `xs` screen size. At the `md` screen size, they will occupy `3:3:3:3` per row, making 3 rows in total. Finally at the `lg` screen size, they will occupy `2:2:2:2:2:2` per row, making 2 rows in total. If you're afraid about cell rows not always adding up to the number 12, fear not. Extra cells just overflow to a new line.

### Adding Gutters

You can add gutters to the grid cells which will create an even, fixed horizontal and vertical spacing between cells, while collapsing the excess amounts. Gutter modifiers come in four sizes:

```html
<div class="Grid Grid--gutter-sm">...</div>
<div class="Grid Grid--gutter-md">...</div>
<div class="Grid Grid--gutter-lg">...</div>
<div class="Grid Grid--gutter-xl">...</div>
```

### Orientation and Spacing

To change the orientation to a columnar grid, add the following modifier:

```html
<div class="Grid Grid--column">...</div>
```

To change the alignment of grid cells with respect to the flex line, use the following modifiers:

```html
<!-- aligns to start of flex line (default) -->
<div class="Grid Grid--top">...</div>

<!-- aligns to end of flex line -->
<div class="Grid Grid--bottom">...</div>

<!-- aligns to center of flex line -->
<div class="Grid Grid--center">...</div>
```

To change the spatial distribution of grid cells along the main axis, use the following modifiers:

```html
<!-- packs cells to center of main axis -->
<div class="Grid Grid--justifyCenter">...</div>

<!-- packs cells to end of main axis -->
<div class="Grid Grid--justifyEnd">...</div>

<!-- distributes extra space evenly between cells -->
<div class="Grid Grid--spaceBetween">...</div>

<!-- distributes extra space evenly around cells -->
<div class="Grid Grid--spaceAround">...</div>
```

## Browser Support

This component has been tested in the following browsers:

* Chrome 46.0+
* Firefox 40.0+
* Safari 9.0+
* Opera 33.0+

If anyone wants to run any tests on older browser versions, please do so and reach out to me!

## Demo

View the live demo at [flexgrid.callmenick.com](http://flexgrid.callmenick.com).

## License (MIT)

Copyright (c) 2016 Nick Salloum

Permission is hereby granted, free of charge, to any person obtaining a copy of this software and associated documentation files (the "Software"), to deal in the Software without restriction, including without limitation the rights to use, copy, modify, merge, publish, distribute, sublicense, and/or sell copies of the Software, and to permit persons to whom the Software is furnished to do so, subject to the following conditions:

The above copyright notice and this permission notice shall be included in all copies or substantial portions of the Software.

THE SOFTWARE IS PROVIDED "AS IS", WITHOUT WARRANTY OF ANY KIND, EXPRESS OR IMPLIED, INCLUDING BUT NOT LIMITED TO THE WARRANTIES OF MERCHANTABILITY, FITNESS FOR A PARTICULAR PURPOSE AND NONINFRINGEMENT. IN NO EVENT SHALL THE AUTHORS OR COPYRIGHT HOLDERS BE LIABLE FOR ANY CLAIM, DAMAGES OR OTHER LIABILITY, WHETHER IN AN ACTION OF CONTRACT, TORT OR OTHERWISE, ARISING FROM, OUT OF OR IN CONNECTION WITH THE SOFTWARE OR THE USE OR OTHER DEALINGS IN THE SOFTWARE.

<p class="text-align--center">
<a href="https://github.com/callmenick/flexgrid.css" class="button button--inline-block button--medium">Get Source</a>
<a href="http://flexgrid.callmenick.com/" class="button button--inline-block button--medium">View Demo</a>
</p>
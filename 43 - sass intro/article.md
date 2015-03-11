## CSS Preprocessors

CSS preprocessors are tools that allow us to code CSS a certain way, and process it into readable, pure CSS. These pure CSS files are what we ultimately need to link to in our projects, as this is the only CSS that browsers will understand. However, preprocessed CSS coding allow us to streamline our work and take on a more scalable approach to our programming. There are a few preprocessors out there, notably Sass (and SCSS), LESS, and Stylus. We’re going to take a plunge into Sass (my preprocessor of choice), see how it works, and look at some examples.

## Sass - Syntactically Awesome Style Sheets

[Sass](http://sass-lang.com/) stands for “Syntactically Awesome Style Sheets”. Sass allows us to streamline our CSS coding by giving us an arsenal of methods at our disposal. In some ways, it’s a step into the world of “object oriented” CSS – we can now extend classes previously defined. We can also nest styles, create functions, set variables, do math, and a host of other goodness. It all sounds amazing, doesn’t it? That’s because…it is.

So what’s all the hype about anyway? Why learn a new language/syntax, when we already know CSS? Why should we have to run a bunch of stuff to make Sass work in the first place, and have to readjust our workflow? These are all common questions we come across before taking the plunge. The truth is though, with a tiny little bit of guidance and effort, you could set yourself up to work fast, boot up projects quickly, and code CSS like a beast. Once you get your workflow in check, and couple that with the fact that Sass already speeds things up for you considerably, you’ll be moving lightning quick and won’t be turning back. Let’s dig deeper.

## Sass & SCSS – What’s The Difference?

Before we get into any examples and learn exactly how Sass works, let’s clarify something that bugs a lot of people. Sass and SCSS are often tossed around interchangeably by people who already know about it. To a newcomer, it can be confusing. [Hugo Giraudel wrote a full article on the difference between Sass and SCSS](http://www.sitepoint.com/whats-difference-sass-scss/), but I’ll cover the basics here.

* Sass (Syntactically Awesome Style Sheets) refers to the preprocessor and syntax as a whole. It takes on its own syntax (the indented syntax), and compiles to readable CSS. We refer to the overall language as Sass.
* SCSS (Sassy CSS) falls under the Sass umbrella. It is a CSS syntax that’s been turbocharged with all the goodness of Sass. Valid CSS is also valid SCSS, so the transition is swift and painless, and you can learn as much as you want at any time to improve your skills.

When I talk about Sass, I mean the Sass umbrella. I actually code with SCSS, and so do many people. It’s the natural step into the world of Sass, because you can dump all your CSS into an SCSS file and compile it perfectly. So, how exactly does all of this work?

## How It All Works

Sass needs to be installed on your machine. It’s a Ruby Gem, which means you need to have Ruby installed on your machine. Ok ok…install something to install something to write some modified CSS. I know it sounds strange and overkill, but bear with me. If you’re using a mac, Ruby is already installed. If not, you can get Ruby installers easily for windows and linux. [You can get the full rundown about installing Sass here](http://sass-lang.com/install). After installing, you’re able to use Sass! If you’r comfortable using the command line, then you’re ready to get going. Now that Sass is installed, you can “watch” a Sass file, and output it to a CSS file on save. More info about all that here.

If you prefer a more visual approach with a GUI, fear not. There are many apps out there to get you started. I personally use the [Compass.app](http://compass.kkbox.com/), which is cheap and awesome. [Codekit}(https://incident57.com/codekit/_ also has great reviews and is very affordable. There are free options too, so the choices are yours. Now that we’re fully covered on all bases, let’s look at some examples.

## Examples

### Variables

We can declare variables in our file that we can reuse throughout our document. This is common for colours and font stacks, and is probably one of the most handy uses of variables off the bat. Here’s the SCSS:

```scss
$pad : 2em;
$color-primary : rgb(40,40,40);
$color-secondary : rgb(40,170,220);

body {
  background-color: $color-primary;
}

h1 {
  color: $color-secondary;
}

.container {
  padding: $pad;
}
```

And here’s the compiled CSS:

```css
body {
  background-color: #282828;
}

h1 {
  color: #28aadc;
}

.container {
  padding: 2em;
}
```

This is the most fundamental approach to using variables, and it’s ridiculously useful in CSS. You can also do math with variables, and there are a host of other functions to get going with. I’ll leave it up to you to research more when you’re ready.

### Nesting

We can nest CSS selectors. This is both beautiful and dangerous, because it’s easy to get carried away with nesting, and end up with very long declarations. Learn it and master it. Nesting can become your best friend, but can also add load to your stylesheet. Let’s look at a classic example – a basic navigation list. Here’s our SCSS:

```scss
nav {
  text-align: center;
  ul {
    list-style: none;
    margin: 0;
    padding: 0;
  }
  li {
    display: inline-block;
  }
  a {
    display: block;
    padding: 5px 10px;
  }
}
```

And this it what it looks like when compiled:

```css
nav {
  text-align: center;
}
nav ul {
  list-style: none;
  margin: 0;
  padding: 0;
}
nav li {
  display: inline-block;
}
nav a {
  display: block;
  padding: 5px 10px;
}
```

Simply awesome. Moving on.

### Extend

We can extend styles from other declarations. This is particularly useful when dealing with CSS grids, and you want to name your styles mathematically in your stylesheet, but keep it all good and semantic in your markup. Let’s look an example. Here’s our SCSS:

```scss
// columns
.col-1-3 {
  width: 33.33%;
}
.col-2-3 {
  width: 66.66%;
}

// colours
.blue {
  color: rgb(40,170,220);
}

// structure
.primary-content {
  @extend .col-2-3;
}
.secondary-content {
  @extend .col-2-3;
}

// text
p.highlight {
  font-size: 3em;
  @extend .blue;
}
```

And here’s the compiled CSS:

```css
.col-1-3, .secondary-content {
 width: 33.33%;
}

.col-2-3, .primary-content {
 width: 66.66%;
}

.blue, p.highlight {
 color: #28aadc;
}

p.highlight {
 font-size: 3em;
}
```

### Import

We can split our CSS into smaller, more maintainable files, and import them into a master stylesheet. We can underscore these files to let Sass know not to compile them into CSS files. Rather, just drop the code into our master file. For example, if we had 3 separate stylesheets for a reset, grid work, and text styles, we would have _reset.scss, _grid.scss, and _text.scss, and our main style.scss would look like this:

```scss
@import 'reset';
@import 'grid';
@import 'text';

body {
}

h1 {
}
```

### Mixins

Mixins are one of the true beauties of Sass. They allow us to create our own functions and pass in variables. This is particularly useful for properties that need vendor prefixing. I’ll snag the example from the Sass website, because it echoes the usage perfectly. Here’s some SCSS:

```scss
@mixin border-radius($radius) {
  -webkit-border-radius: $radius;
     -moz-border-radius: $radius;
      -ms-border-radius: $radius;
          border-radius: $radius;
}

.box {
  @include border-radius(10px);
}
```

And here’s our perfectly compiled CSS:

```css
.box {
  -webkit-border-radius: 10px;
  -moz-border-radius: 10px;
  -ms-border-radius: 10px;
  border-radius: 10px;
}
```

## Wrap Up

This is just the tip of the iceberg that is the world of Sass. I recommend digging into the [Sass reference](http://sass-lang.com/documentation/file.SASS_REFERENCE.html) to learn more. There’s a lot of cool stuff to be done, and the learning curve is entirely up to you. Remember, valid CSS is also perfectly valid SCSS, so you’re ready to start right this second. After you wrap your head around it all a bit, I highly recommend looking into a CSS authoring framework like [Compass](http://compass-style.org/). These frameworks come chock full of mixins and configuration set ups to streamline your workflow even further. Take it step by step, and get set up in a way that’s comfortable with you. Thanks for reading, and feel free to leave any comments, input, and feedback below!
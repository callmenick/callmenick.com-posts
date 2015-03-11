<p class="text-align--center">
<a href="http://callmenick.com/development/transitions-transforms-animations/1-flipping-card/1-flipping-card-source.zip" class="button button--inline-block button--medium">Get Source</a>
<a href="http://callmenick.com/development/transitions-transforms-animations/1-flipping-card/" class="button button--inline-block button--medium">View Demo</a>
</p>

This is part 1 of a series of tutorials about practical use cases of CSS transitions, transforms, and animations. In this tutorial, we'll look at a "flipping card" scenario, and implementation variations.

## The Flipping Card

Flipping cards or tiles can be very useful these days. First of all, the provide users with a bit of interactivity, keeping them engaged, and these days engagement is key. The most common use-case for flipping cards and tiles that I can think of would be including some kind of imagery on the front, and some info on the back. We will ultimately need two different element wrappers contained inside a parent element, but the elements you decide is up to you. For the sake of this tutorial, I'll be using generic `div`s. First, let's look at the three effects we'll implement:

1. On hover - flip transition induced when a user hovers on the card.
2. On click - flip transition induced when a user clicks on the card.
3. Random - flip transition induced randomly.

Now, let's dig in.

## The Markup

As I mentioned above, the markup generally consists of a parent element and two child elements. Each child represents the front and back of the card. Here's the markup we'll use for each card:

```html
<div class="card effect__EFFECT">
  <div class="card__front">
    <span class="card__text">front</span>
  </div>
  <div class="card__back">
    <span class="card__text">back</span>
  </div>
</div><!-- /card -->
```

Notice the "EFFECT" text in the parent element. This will be either `hover`, `click`, or `random` depending on the effect we're trying to achieve. Let's first discuss the layout, the CSS we'll want to achieve, and the available properties that we can leverage.

## How To Achieve What We Want To Achieve

In order to have an effective flipping card animation/transition, the front-card element needs to initially be stacked on top the back-card element. The back-card element also needs to be initially hidden from view. When the transition is induced, the front and back of the cards need to simultaneously rotate in and out of view around the y-axis. CSS transforms can happen in any of the 3-dimensions we're familiar with. If you're looking at your computer screen, place an imaginary point in the centre of it. Here's the axis orientation -

* x-axis: drawn from that point horizontally
* y-axis: drawn from that point upwards
* z-axis: drawn from that point into the screen.

Rotating around the x- or y-axis will give us the desired flipping effect, but for the safe of this tutorial, we'll use the y-axis.

For the cards to be stacked, we'll have to leverage `absolute` positioning. To keep them pinned to our parent container, we'll have to use `relative` positioning on it. With that in mind, we'll need to explicitly set widths and heights, or let at least one card's dimensions be dictated by some media (like an image). In the case of the card dimensions being governed by an image object, only one of the cards would need to be absolutely positioned.

Another thing to note is that if there is any transparency in the cards (i.e. no solid backgrounds, images, or colours), we will need to hide the "backs" of cards, so that a flipped version of it doesn't show up on the one in front of it. For this, we'll use the CSS property `backface-visiblity` and set it to `hidden`.

Finally, for this tutorial, I'll use the padded-parent technique to achieve some perfectly square tiles.

## Some Common CSS

Let's take a look at some common CSS that will be applicable across all three implementations:

```css
.card {
  position: relative;
  float: left;
  padding-bottom: 25%;
  width: 25%;
  text-align: center;
}

.card__front,
.card__back {
  position: absolute;
  top: 0;
  left: 0;
  width: 100%;
  height: 100%;
}

.card__front,
.card__back {
  -webkit-backface-visibility: hidden;
          backface-visibility: hidden;
  -webkit-transition: -webkit-transform 0.3s;
          transition: transform 0.3s;
}

.card__front {
  background-color: #ff5078;
}

.card__back {
  background-color: #1e1e1e;
  -webkit-transform: rotateY(-180deg);
          transform: rotateY(-180deg);
}
```

I'll be using four tiles per row, hence the width of 25%. To make a perfectly square tile container, I'll also set the bottom padding to 25%. This will allow us to position the fronts and backs absolutely, and any content underneath with follow the document flow. Also of importance is the transition property. We're applying a `transition` to the `transform`, as the transform property is what changes. The back is initially set to be rotated -180 degrees around the x-axis, and the front stays as default. Now, let's look at each of the implementations.

## Effect 1 - Transition On Hover

This one is simple, and we can use the CSS pseudo-class `:hover` to achieve the effect. When we hover over the card, the front should transition to -180 degrees, and the back should transition to 0. Here's the CSS used to achieve that:

```css
.card.effect__hover:hover .card__front {
  -webkit-transform: rotateY(-180deg);
          transform: rotateY(-180deg);
}

.card.effect__hover:hover .card__back {
  -webkit-transform: rotateY(0);
          transform: rotateY(0);
}
```
Pretty simple, huh? On to number 2.

## Effect 2 - Transition On Click

This time, we want the transition induced on click. We'll leverage a little javascript to achieve this, because we'll need to check classes and whatnot to make sure we can actually un-flip the card on a second click. Here's the CSS:

```css
.card.effect__click.flipped .card__front {
  -webkit-transform: rotateY(-180deg);
          transform: rotateY(-180deg);
}

.card.effect__click.flipped .card__back {
  -webkit-transform: rotateY(0);
          transform: rotateY(0);
}
```

And here's the JavsScript used:

```javascript
(function() {
  var cards = document.querySelectorAll(".card.effect__click");
  for ( var i  = 0, len = cards.length; i < len; i++ ) {
    var card = cards[i];
    clickListener( card );
  }

  function clickListener(card) {
    card.addEventListener( "click", function() {
      var c = this.classList;
      c.contains("flipped") === true ? c.remove("flipped") : c.add("flipped");
    });
  }
})();
```

Nice! Now, onto the third implementation.

## Effect 3 - Randomly Inducing The Transition

This one is sort of a bonus, and requires some extras. Because we're going to randomly induce the transition at random times, we're going to need multiple unique timeouts in out JavaScript. For this, we'll use some data-attributes in our markup too. Here's the markup:

```html
<div class="card effect__random" data-id="1">
  <div class="card__front">
    <span class="card__text">front</span>
  </div>
  <div class="card__back">
    <span class="card__text">back</span>
  </div>
</div><!-- /card -->
```

The CSS is pretty much the same as before too, but here it is anyway:

```css
.card.effect__random.flipped .card__front {
  -webkit-transform: rotateY(-180deg);
          transform: rotateY(-180deg);
}

.card.effect__random.flipped .card__back {
  -webkit-transform: rotateY(0);
          transform: rotateY(0);
}
```

And now, for some nifty JavaScript. I set up a range between 1 and 3 seconds, which will randomly generate a time delay for cards to flip. The timeout will get cleared each time, and reset, creating a purely visual effect. Here's the JavaScript:

```javascript
(function() {

  // cache vars
  var cards = document.querySelectorAll(".card.effect__random");
  var timeMin = 1;
  var timeMax = 3;
  var timeouts = [];

  // loop through cards
  for ( var i = 0, len = cards.length; i < len; i++ ) {
    var card = cards[i];
    var cardID = card.getAttribute("data-id");
    var id = "timeoutID" + cardID;
    var time = randomNum( timeMin, timeMax ) * 1000;
    cardsTimeout( id, time, card );
  }

  // timeout listener
  function cardsTimeout( id, time, card ) {
    if (id in timeouts) {
      clearTimeout(timeouts[id]);
    }
    timeouts[id] = setTimeout( function() {
      var c = card.classList;
      var newTime = randomNum( timeMin, timeMax ) * 1000;
      c.contains("flipped") === true ? c.remove("flipped") : c.add("flipped");
      cardsTimeout( id, newTime, card );
    }, time );
  }

  // random number generator given min and max
  function randomNum( min, max ) {
    return Math.random() * (max - min) + min;
  }

})();
```

Fun stuff!

## Flipping Cards In Action

There's a lot of flipping card implementations around the web. In particular, I've seen a lot of gallery layouts and portfolio sites using variations of it. But it's not limited to just things like this. A few weeks ago, I built an HTML5 memory game, and made heavy use of this technique. You can [check out the game here](http://callmenick.com/memory/), and the source code is also [available on GitHub](https://github.com/callmenick/Memory/).

## Browser Support

This technique makes heave use of CSS transitions and transforms, so take that into account when building your site or app. Modernizr is a great feature detection tool, so provide a suitable fallback when transitions / transforms aren't available. Think along the lines of swapping z-index values depending on whether front or back is being shown.

## Wrap Up

And that’s a wrap! We covered some important features here, and took a little plunge into the world of CSS3 transitions and transforms, and looked at a real-world use case for them. Don’t forget, you can view the demo and download the source by clicking the links below, and if you have and questions, comments, or feedback, you can also leave them below.

<p class="text-align--center">
<a href="http://callmenick.com/development/transitions-transforms-animations/1-flipping-card/1-flipping-card-source.zip" class="button button--inline-block button--medium">Get Source</a>
<a href="http://callmenick.com/development/transitions-transforms-animations/1-flipping-card/" class="button button--inline-block button--medium">View Demo</a>
</p>
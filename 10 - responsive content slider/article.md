In this lab, I’m going to make a responsive content slider using HTML, CSS, and Javascript (jQuery). It’ll be a jQuery plugin, and will be able to work on any div that takes on a certain markup. A content slider is a great way of displaying linear information in a neat, concise manner. Enjoy!

## Getting Started

To start, let’s conceptualise exactly what we’re trying to achieve. We want a series of slides, laid out from left to right, containing content that reads linearly. We want previous and next buttons to trigger the slider to slide left or right, and we want to disable the next button if we’re on the last slide, and disable the previous button if we’re on the first slide. We also want this to be responsive, adapting to any width/height. Let’s get into some markup. I’ve prefixed all my id’s and classes with `cslide` so that it doesn’t conflict with any other styles you may have in your stylesheet.

The markup will go as follows -

* We want a containing div with a unique id, that the plugin will work on
* This div will house the previous button, next button, and the slides
* The slides themselves will be nested inside another container

This is what our markup will look like -

```language-markup
<section id="cslide-slides" class="cslide-slides-master clearfix container">
    <div class="cslide-prev-next clearfix">
        <span class="cslide-prev">prev slide</span>
        <span class="cslide-next">next slide</span>
    </div>
    <div class="cslide-slides-container clearfix">
        <div class="cslide-slide">
            <h2>This is slide 1</h2>
            <p>Lorem ipsum dolor sit amet, consectetuer adipiscing elit....</p>
        </div>
        <div class="cslide-slide">
            <h2>This is slide 2</h2>
            <p>Lorem ipsum dolor sit amet, consectetuer adipiscing elit....</p>
        </div>
        <div class="cslide-slide">
            <h2>This is slide 3</h2>
            <p>Lorem ipsum dolor sit amet, consectetuer adipiscing elit....</p>
        </div>
        <div class="cslide-slide">
            <h2>This is slide 4</h2>
            <p>Lorem ipsum dolor sit amet, consectetuer adipiscing elit....</p>
        </div>
        <div class="cslide-slide">
            <h2>This is slide 5</h2>
            <p>Lorem ipsum dolor sit amet, consectetuer adipiscing elit....</p>
        </div>
    </div>
</section><!-- /sliding content section -->
```

## Notes About The Markup

For our plugin to work, the markup must follow a structure. We want all our slides to exist side by side in a parent container, `#cslide-slides`, and each slide to exist in a class `.cslide-slide`. Inside each of these slides, we can place whatever content we want. The idea behind this markup is this -

* we want our parent container of all the slides to occupy the width of its parent
* in this case, it’s responsive, so we set it to 100% width, and the parent div (wrapper, for instance) is 80% fluid width.
* each of the slides will sit side by side, in a left-to-right order, but only one slide will show at a time.
* as we hit the previous and next buttons, the previous or next slide will slide in from the left or right
* if we’re on the first slide, we want the previous button disabled
* if we’re on the last slide, we want the next button disabled

## The CSS

Now, let’s dig into our CSS a bit. Remembering what we just looked at above, here’s what our CSS might look like. Remember, I’m using the box-sizing:border-box property (with the necessary prefixes and polyfills) throughout my stylesheet for easy padded divs.

```language-css
#cslide-slides h2 {
	margin-bottom:10px;
	font-weight: 700;
}
/*=Slides
----------------------------------------------- */
#cslide-slides {

}
.cslide-slides-master {
	overflow: hidden;
	margin-bottom:60px;
}
.cslide-slides-master:last-child {
	margin-bottom:0;
}
.cslide-slides-container {
	visibility: hidden;
}
.cslide-slide {
	float:left;
	padding:30px;
	border:solid 10px rgb(150,30,50);
	background-color:rgb(200,80,100);
}
.cslide-slide h2,
.cslide-slide p {
	color:#fff;
}
/* prev next buttons */
.cslide-prev-next {
	margin-bottom: 30px;
	display: none;
	text-align: right;

	-webkit-touch-callout: none;
	-webkit-user-select: none;
	-khtml-user-select: none;
	-moz-user-select: none;
	-ms-user-select: none;
	user-select: none;
}
.cslide-prev,.cslide-next {
	display:inline-block;
	background:rgb(100,200,150);
	color:#fff;
	padding:10px 20px;

	border-bottom:solid 5px rgb(50,150,100);

	cursor: pointer;
}
.cslide-prev:hover,
.cslide-next:hover {
	background:rgb(75,175,125);
}
.cslide-prev.cslide-disabled:hover,
.cslide-next.cslide-disabled:hover {
	background:rgb(100,200,150);
}
.cslide-disabled {
	opacity: 0.3;
}
```

Everything here is fairly straightforward. You may be wondering why we didn’t apply widths to any of the slides, or the container. These dimensions are calculated with javascript, depending on the number of slides. Some point to note here are -

* `.cslide-slides-master` has an overflow set to hidden, in order to hide slides that are outside the container
* `.cslide-slides-container` initially has the visibility property set to hidden in order to not show a disorderly looking slider
* `.cslide-slide` is floated to the left with some basic styling, but has no width (the width is calculated depending on how many slides you have)
* everything else is basic css properties to give you a starting point

## Diving Into The JS

This is where the slideshow ticks. We’re going to make a jQuery function that we can apply to any div that has the above markup. In our case, that div is `#cslide-slides`, and we want to call our function on it like this -

```language-javascript
$(document).ready(function(){
    $("#cslide-slides").cslide();
});
```

Before we get into our JS, let’s take a look at the code. We’re creating a function, and calling it `cslide`. This function calculates the number of slides, sets the parent width, sets the slide width, then sets the visibility of the slides container. Then, it handles the clicking of previous and next buttons, disabling them when necessary. Because we always start on slide 1, the previous button is disabled from the start. Here’s what the code for our function looks like -

```language-javascript
(function($) {

    $.fn.cslide = function() {

        this.each(function() {

            var slidesContainerId = "#"+($(this).attr("id"));

            var len = $(slidesContainerId+" .cslide-slide").size();     // get number of slides
            var slidesContainerWidth = len*100+"%";                     // get width of the slide container
            var slideWidth = (100/len)+"%";                             // get width of the slides

            // set slide container width
            $(slidesContainerId+" .cslide-slides-container").css({
                width : slidesContainerWidth,
                visibility : "visible"
            });

            // set slide width
            $(".cslide-slide").css({
                width : slideWidth
            });

            // add correct classes to first and last slide
            $(slidesContainerId+" .cslide-slides-container .cslide-slide").last().addClass("cslide-last");
            $(slidesContainerId+" .cslide-slides-container .cslide-slide").first().addClass("cslide-first cslide-active");

            // initially disable the previous arrow cuz we start on the first slide
            $(slidesContainerId+" .cslide-prev").addClass("cslide-disabled");

            // if first slide is last slide, hide the prev-next navigation
            if (!$(slidesContainerId+" .cslide-slide.cslide-active.cslide-first").hasClass("cslide-last")) {           
                $(slidesContainerId+" .cslide-prev-next").css({
                    display : "block"
                });
            }

            // handle the next clicking functionality
            $(slidesContainerId+" .cslide-next").click(function(){
                var i = $(slidesContainerId+" .cslide-slide.cslide-active").index();
                var n = i+1;
                var slideLeft = "-"+n*100+"%";
                if (!$(slidesContainerId+" .cslide-slide.cslide-active").hasClass("cslide-last")) {
                    $(slidesContainerId+" .cslide-slide.cslide-active").removeClass("cslide-active").next(".cslide-slide").addClass("cslide-active");
                    $(slidesContainerId+" .cslide-slides-container").animate({
                        marginLeft : slideLeft
                    },250);
                    if ($(slidesContainerId+" .cslide-slide.cslide-active").hasClass("cslide-last")) {
                        $(slidesContainerId+" .cslide-next").addClass("cslide-disabled");
                    }
                }
                if ((!$(slidesContainerId+" .cslide-slide.cslide-active").hasClass("cslide-first")) && $(".cslide-prev").hasClass("cslide-disabled")) {
                    $(slidesContainerId+" .cslide-prev").removeClass("cslide-disabled");
                }
            });

            // handle the prev clicking functionality
            $(slidesContainerId+" .cslide-prev").click(function(){
                var i = $(slidesContainerId+" .cslide-slide.cslide-active").index();
                var n = i-1;
                var slideRight = "-"+n*100+"%";
                if (!$(slidesContainerId+" .cslide-slide.cslide-active").hasClass("cslide-first")) {
                    $(slidesContainerId+" .cslide-slide.cslide-active").removeClass("cslide-active").prev(".cslide-slide").addClass("cslide-active");
                    $(slidesContainerId+" .cslide-slides-container").animate({
                        marginLeft : slideRight
                    },250);
                    if ($(slidesContainerId+" .cslide-slide.cslide-active").hasClass("cslide-first")) {
                        $(slidesContainerId+" .cslide-prev").addClass("cslide-disabled");
                    }
                }
                if ((!$(slidesContainerId+" .cslide-slide.cslide-active").hasClass("cslide-last")) && $(".cslide-next").hasClass("cslide-disabled")) {
                    $(slidesContainerId+" .cslide-next").removeClass("cslide-disabled");
                }
            });

        });

        // return this for chainability
        return this;

    }

}(jQuery));
```

Here’s the order in which our function is performing -

* fetch the slide container id
* get the number of slides based on number of classes `.cslide-slide` that exist
* set the slide container width and the slide width based on the number of slides, and change the visibility of the slide container to visible
* add classes of `cslide-last`  to the last slide, and `cslide-first` and `cslide-active` to the first slide
* disable the previous button because by default we’re on the first slide
* show the previous and next buttons if we have more than one slide, else leave the display:none  property from the css untouched
* handle the clicking of the previous and next slide buttons, sliding the container to the left or right by the appropriate amount
* use jQuery animate to animate the margin, giving the slides a slide left and slide right appearance
* make sure the previous or next button gets disabled if on the first or last slide

## Wrap Up

And there you have it. A neat jQuery function that allows  you to create as many responsive content sliders as you want, as long as they have unique id’s and follow a certain markup.
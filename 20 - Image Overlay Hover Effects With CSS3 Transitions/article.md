<p class="text-align--center">
<a href="http://www.callmenick.com/_development/image-overlay-hover-effects/image-overlay-hover-effects-source.zip" class="button button--inline-block button--medium">Get Source</a>
<a href="http://www.callmenick.com/_development/image-overlay-hover-effects" class="button button--inline-block button--medium">View Demo</a>
</p>

Image overlay hover effects with CSS3 transitions are a great way to add some nice interactivity on your site. In the old days, a little pointer cursor would do the trick for indicating to the user that an image was clickable to view more information. Nowadays, we want some smooth transitions and some UI icons to prompt the user. In our case, we’re going to have a series of images. When the user hovers over an image, a semi-transparent black background transitions in over the image, along with a “+” icon. This will indicate to the user that they can click the image for things like more info, a larger view of the image, etc. Let’s get started!

## Diving In

For the tutorial post, I’m only going to explain and go through one overlay hover effect. If you get the source though, you’ll have full access to all the CSS/markup that covers all 6 transitions in the demo. Without further ado, let’s look at the HTML markup for one of the transitions.

```html
<div id="effect-1" class="effects clearfix">
    <div class="img">
        <img src="img/jpg/1.jpg" alt="">
        <div class="overlay">
            <a href="#" class="expand">+</a>
            <a class="close-overlay hidden">x</a>
        </div>
    </div>
    <div class="img">
        <img src="img/jpg/2.jpg" alt="">
        <div class="overlay">
            <a href="#" class="expand">+</a>
            <a class="close-overlay hidden">x</a>
        </div>
    </div>
    <div class="img">
        <img src="img/jpg/3.jpg" alt="">
        <div class="overlay">
            <a href="#" class="expand">+</a>
            <a class="close-overlay hidden">x</a>
        </div>
    </div>
    <div class="img">
        <img src="img/jpg/4.jpg" alt="">
        <div class="overlay">
            <a href="#" class="expand">+</a>
            <a class="close-overlay hidden">x</a>
        </div>
    </div>
</div>
```

## Notes About The HTML

The markup is straightforward. For the case of the tutorial, I demonstrated 6 different effects. Each effect is structured inside a parent container with a class of `.effects`. This allows us to target all the images inside this div. Each image with overlay link is placed inside a div  with a class of `.img`. The parent container will have a relative positioning, allowing us to give our overlay an absolute position that spans the width and height of the container (i.e. the space occupied by the image).

Instead of using the CSS pseudo class :hover, we’re going to check it with JavaScript/jQuery and Modernizr. This will enable us to induce the same effect on hover OR on click. Modernizr allows us to check if touch is enabled (which in most cases means that we’re on a touch screen device). If it is, a click/touch will induce the effect. If not, a hover will induce the effect. We’ll use jQuery’s mousenter/mouseleave functionality in this case to toggle the effect. The toggling of the effect will be induced by adding/removing the class `.hover` to our image.

In the case of touch devices, we want to give the user the option of hiding the overlay. We’ll present a little close button inside the overlay that defaults to a hidden display. If touch is sensed by Modernizr though, we’ll show the close button, which will close the overlay. This isn’t necessary for hovering, because when the mouse leaves the area, the overlay will transition out.

First, let’s take a look at all the common CSS:

```css
/* ============================================================
  GLOBAL
============================================================ */
.effects {
  padding-left: 15px;
}
.effects .img {
  position: relative;
  float: left;
  margin-bottom: 5px;
  width: 25%;
  overflow: hidden;
}
.effects .img:nth-child(n) {
  margin-right: 5px;
}
.effects .img:first-child {
  margin-left: -15px;
}
.effects .img:last-child {
  margin-right: 0;
}
.effects .img img {
  display: block;
  margin: 0;
  padding: 0;
  max-width: 100%;
  height: auto;
}

.overlay {
  display: block;
  position: absolute;
  z-index: 20;
  background: rgba(0, 0, 0, 0.8);
  overflow: hidden;
  transition: all 0.5s;
}

a.close-overlay {
  display: block;
  position: absolute;
  top: 0;
  right: 0;
  z-index: 100;
  width: 45px;
  height: 45px;
  font-size: 20px;
  font-weight: 700;
  color: #fff;
  line-height: 45px;
  text-align: center;
  background-color: #000;
  cursor: pointer;
}
a.close-overlay.hidden {
  display: none;
}

a.expand {
  display: block;
  position: absolute;
  z-index: 100;
  width: 60px;
  height: 60px;
  border: solid 5px #fff;
  text-align: center;
  color: #fff;
  line-height: 50px;
  font-weight: 700;
  font-size: 30px;
  border-radius: 30px;
}
```

Nice! We’ve styled our overlay, our link button, and our close overlay button. Now, let’s look at the CSS for the Slide In Bottom effect, i.e. effect 1:

```css
/* ============================================================
  EFFECT 1 - SLIDE IN BOTTOM
============================================================ */
#effect-1 .overlay {
  bottom: 0;
  left: 0;
  right: 0;
  width: 100%;
  height: 0;
}
#effect-1 .overlay a.expand {
  left: 0;
  right: 0;
  bottom: 50%;
  margin: 0 auto -30px auto;
}
#effect-1 .img.hover .overlay {
  height: 100%;
}
```

## Notes About The CSS

Each image and overlay is wrapped in a relatively positioned div. This allows us to position the overlay absolutely in relation to the parent. For effect 1, the slide in bottom effect, we give our overlay a width of 100% and a height of 0. We also set the overflow to hidden, position it at the bottom of the parent div, and give it a semi-transparent background. Finally, we apply the transition property. We also created a nice “+” icon that might hint to the user to click for more info. This is positioned absolutely in the center of the overlay, and will transition with the overlay. We give our parent div an overflow property set to hidden, and finally, when the hover class is added to the parent, our overlay height property gets set to 100%. This will transition the overlay into view nicely. Also, note that the close-overlay link is set to `dispaly:none` if a class of hidden is applied to it. We will show this button by removing this class if Modernizr detects touch.

Now, let’s look at the JS that will bring our transitions to life on hover or on click, depending on what device the user is on. Remember, you must include Modernizr to detect touch as much as possible. This isn’t however a bulletproof method, but it covers quite a lot.

```javascript
$(document).ready(function(){
    if (Modernizr.touch) {
        // show the close overlay button
        $(".close-overlay").removeClass("hidden");
        // handle the adding of hover class when clicked
        $(".img").click(function(e){
            if (!$(this).hasClass("hover")) {
                $(this).addClass("hover");
            }
        });
        // handle the closing of the overlay
        $(".close-overlay").click(function(e){
            e.preventDefault();
            e.stopPropagation();
            if ($(this).closest(".img").hasClass("hover")) {
                $(this).closest(".img").removeClass("hover");
            }
        });
    } else {
        // handle the mouseenter functionality
        $(".img").mouseenter(function(){
            $(this).addClass("hover");
        })
        // handle the mouseleave functionality
        .mouseleave(function(){
            $(this).removeClass("hover");
        });
    }
});
```

## Some Notes About The JavaScript/jQuery

We first use Modernizr to detect touch. If we have touch, then we trigger our functions via click . If we don’t have touch, then we trigger our functions via hover using the `mouseenter` and `mouseleave` functions from jQuery.

For touch, we first display the close-overlay button. Then, when the user clicks, we add the class of .hover to the `.img` container. This transitions in the overlay, and the elements placed inside it (the expand button and the close-overlay button). The only way to close the overlay is to click the close-overlay button, so we register this functionality similarly. This time, it removes the class of hover, transitioning the overlay out.

For mouse devices, we use the simple `mouseenter` and `mouseleave` functions to add and remove the hover classes to the div. This is a straightforward method, so doesn’t require much further explanation.

## Wrap Up

Using the above techniques, the transition of the overlay can happen any way you want. I’ve included 6 different transitions in the demo, which you can get via downloading the source by clicking the links below. I highly encourage you to play around with the transitions, exploring different methods and techniques. In the source, there’s also some media queries to help you get started. Feel free to leave any comments or feedback below!

Special thanks to [Marco Goran Romano](http://dribbble.com/goranfactory) for the sweet imagery.


<p class="text-align--center">
<a href="http://www.callmenick.com/_development/image-overlay-hover-effects/image-overlay-hover-effects-source.zip" class="button button--inline-block button--medium">Get Source</a>
<a href="http://www.callmenick.com/_development/image-overlay-hover-effects" class="button button--inline-block button--medium">View Demo</a>
</p>
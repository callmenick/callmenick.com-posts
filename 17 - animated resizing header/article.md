<p class="text-align--center">
<a href="http://callmenick.com/tutorial-demos/resize-header-on-scroll/resize-header-on-scroll.zip" class="button button--inline-block button--medium">Get Source</a>
<a href="http://callmenick.com/tutorial-demos/resize-header-on-scroll/" class="button button--inline-block button--medium">View Demo</a>
</p>

We’re going to create an animated resizing header on scroll. An animated header that resizes on scroll is a neat touch to sites that have simple navigation structures. The animated resizing functionality, combined with the fixed header positioning, gives a smooth experience to the user while allowing more viewing space on the screen as the user scrolls down a bit. Let’s get started by looking at what we need to achieve our goal.

## Getting Started

To start with, it’s important to note that this functionality requires a `fixed` position header. For browsers that don’t support  fixed positioning, or are buggy in its implementation, you need to consider a fallback. To check compatibility, visit [Can I Use](http://caniuse.com/). I also used the classie.js javascript library for quick adding/removing/checking of classes on elements in the DOM. Let’s now take a look at our HTML markup. The markup consists of a header with a logo and navigation, some sections of text, and a footer.

```html
<header>
    <div class="container clearfix">
        <h1 id="logo">
            LOGO
        </h1>
        <nav>
            <a href="">Lorem</a>
            <a href="">Ipsum</a>
            <a href="">Dolor</a>
        </nav>
    </div>
</header><!-- /header -->
```

## Some Notes About The HTML

The HTML is fairly straightforward. We have a logo and navigation menu inside a header tag. It’s up to you of course how you structure your header…this is just for display purposes. Now, let’s get into the CSS.

```css
header {
    width: 100%;
    height: 150px;
    overflow: hidden;
    position: fixed;
    top: 0;
    left: 0;
    z-index: 999;
    background-color: #0683c9;
    -webkit-transition: height 0.3s;
    -moz-transition: height 0.3s;
    -ms-transition: height 0.3s;
    -o-transition: height 0.3s;
    transition: height 0.3s;
}
header h1#logo {
    display: inline-block;
    height: 150px;
    line-height: 150px;
    float: left;
    font-family: "Oswald", sans-serif;
    font-size: 60px;
    color: white;
    font-weight: 400;
    -webkit-transition: all 0.3s;
    -moz-transition: all 0.3s;
    -ms-transition: all 0.3s;
    -o-transition: all 0.3s;
    transition: all 0.3s;
}
header nav {
    display: inline-block;
    float: right;
}
header nav a {
    line-height: 150px;
    margin-left: 20px;
    color: #9fdbfc;
    font-weight: 700;
    font-size: 18px;
    -webkit-transition: all 0.3s;
    -moz-transition: all 0.3s;
    -ms-transition: all 0.3s;
    -o-transition: all 0.3s;
    transition: all 0.3s;
}
header nav a:hover {
    color: white;
}
header.smaller {
    height: 75px;
}
header.smaller h1#logo {
    width: 150px;
    height: 75px;
    line-height: 75px;
    font-size: 30px;
}
header.smaller nav a {
    line-height: 75px;
}

@media all and (max-width: 660px) {
    header h1#logo {
        display: block;
        float: none;
        margin: 0 auto;
        height: 100px;
        line-height: 100px;
        text-align: center;
    }
    header nav {
        display: block;
        float: none;
        height: 50px;
        text-align: center;
        margin: 0 auto;
    }
    header nav a {
        line-height: 50px;
        margin: 0 10px;
    }
    header.smaller {
        height: 75px;
    }
    header.smaller h1#logo {
        height: 40px;
        line-height: 40px;
        font-size: 30px;
    }
    header.smaller nav {
        height: 35px;
    }
    header.smaller nav a {
        line-height: 35px;
    }
}
```

## Some Notes About The CSS

To begin, the CSS is standard. We have a fixed header that is stuck to the top of the page using top and left. It’s width is set to 100%, and I’ve given it a height of 150px. Our `z-index` keeps it above everything else, and we also give it a CSS3 transition property so that when it resizes, it does so smoothly. We’ll add a `smaller` class definition in our CSS also, and under this, our header will have its resized properties. When the header has the class of `smaller`, it will resize. This class will be added on scroll (as we’ll see in the javascript later).

The logo and navigation items are given a line height of the header’s height (i.e. 150px) to start. When the header gets resized on scroll, we’ll shrink these values to 75px. We will also change the font size of the logo to something smaller to fit the available space.

Finally, I added some example media queries so that our animated resizing header will work across all devices and screen sizes. Now, let’s make the magic happen. We’re going to use javascript to add and remove the class smaller when we scroll a certain amount. Adding and removing this class will animate our header.

```javascript
function init() {
    window.addEventListener('scroll', function(e){
        var distanceY = window.pageYOffset || document.documentElement.scrollTop,
            shrinkOn = 300,
            header = document.querySelector("header");
        if (distanceY > shrinkOn) {
            classie.add(header,"smaller");
        } else {
            if (classie.has(header,"smaller")) {
                classie.remove(header,"smaller");
            }
        }
    });
}
window.onload = init();
```

## Some Notes About The Javascript

In our JS, we look out for a scroll event. We set a distance of 300px to be our marker for when our header will resize and animate. If the scroll distance in the Y direction is greater than this value, we add the class `smaller` to the `header`. If it’s less than or equal to this amount, we remove the class of `smaller` if the `header` has the `smaller` class. When the window has loaded, we run the function.

## Wrap Up

That’s a wrap! Of course, it’s up to you to change the sizes, dimensions, and content of your header and implement it into your site. But with the knowledge presented here, you can easily implement this animated resizing header into your next project. Thanks for checking it out, and feel free to view the demo, download the source, and leave any comments below.

<p class="text-align--center">
<a href="http://callmenick.com/tutorial-demos/resize-header-on-scroll/resize-header-on-scroll.zip" class="button button--inline-block button--medium">Get Source</a>
<a href="http://callmenick.com/tutorial-demos/resize-header-on-scroll/" class="button button--inline-block button--medium">View Demo</a>
</p>
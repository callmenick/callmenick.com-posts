<p class="text-align--center">
<a href="http://callmenick.com/_development/single-page-smooth-scroll/single-page-smooth-scroll-soure.zip" class="button button--inline-block button--medium">Get Source</a>
<a href="http://callmenick.com/_development/single-page-smooth-scroll" class="button button--inline-block button--medium">View Demo</a>
</p>

In this lab, I’m going to demonstrate how to create a fixed navigation, single page site with smooth scrolling, and highlighting the navigation link depending on which section is currently being viewed. This technique is great for single page sites that want to pack in lots of goodies on one page, but use a navigation with cool scrolling to section it off. Let’s get started.

## Getting Started With The Markup

The markup is fairly easy. We’re going to use a standard two column floated display. The navigation will be in the left sidebar, and the main content will be in the right content column. Here’s a look at the html.

```html
<div id="main">
    <div class="container clearfix">        
        <div id="sidebar">
            <div id="nav-anchor"></div>
            <nav>
                <ul>
                    <li><a href="#home">Home</a></li>
                    <li><a href="#about">About</a></li>
                    <li><a href="#services">Services</a></li>
                    <li><a href="#blog">Blog</a></li>
                    <li><a href="#contact">Contact</a></li>
                </ul>
            </nav>
        </div><!-- /sidebar -->
        <div id="content">
            <section id="home">
                ...
            </section>
            <section id="about">
                ...
            </section>
            <section id="services">
                ...
            </section>
            <section id="blog">
                ...
            </section>
            <section id="contact">
                ...
            </section>
        </div><!-- /#content -->
    </div><!-- /.container -->
</div><!-- /#main -->
```

## Now, Let’s Style With CSS

Using standard CSS methods, we create a two column layout. Looking at the HTML, we see that our navigation is nested in a `nav` tag, and inside that tag is an unordered list. Inside our content `div`, we separate the main sections of content using `section` tags. There are a few extra CSS classes that we will declare. One is `.nav-active`, which we will use to highlight the navigation item that is currently being viewed. Also, `nav.stick` will be used to pin our navigation to the top of the page after scrolling. Note, I also used `box-sizing:border-box` on all elements in a master style sheet. Our CSS should look something like this.

```css
#sidebar {
    width:300px;
    float:left;
}

#content {
    width:724px;
    float:right;
    padding-left:24px;
}

/* navigation */
nav {
    width:300px;
    background-color:rgb(240,240,240);
    border:solid 1px rgb(220,220,220);
    padding:0 12px;
}

nav.stick {
    position: fixed;
    top: 0;
    z-index: 10000;
    margin-top:12px;
}

nav ul {
    list-style-type:none;
    margin:0;
    padding:0;
}

nav li {

}

nav li a {
    color:rgb(50,50,50);
    font-weight:700;
}

a.nav-active {
    color:#ccc;
}
```

## Digging Into the JQuery and JavaScript

First, let’s make our navigation box fixed after scrolling. When the navigation hits the top of the window, we want it to become fixed. Using an empty tag with an id of `nav-anchor` placed right above our navigation, we can find the height from the top of the window to the tag. Using the jQuery function `scroll()`, we can update a variable that gives us the distance of the window to the top of the page. Then, if this distance becomes greater than the distance initially calculated (from the div to the top), we add the class `.stick` to the element.

Next, using the JQuery plugin [scrollto.js](http://flesler.blogspot.com/2007/10/jqueryscrollto.html), we apply smooth scrolling to our page. This is easy to use, and well documented on the site.

Now for the fun part. We create an array of all our `nav a href’s`. We then use some calculations using the scroll function. We find the `section id`, calculate it’s height, see if it’s greater or less than the value from the window top, and if the window falls in between, we add a class `nav-active` to the list item in question. We create a conditional also, because if the top of a section is not reached and the page can’t scroll anymore, we want to highlight this section. Here’s what our JavaScript looks like.

```javascript
$(document).ready(function(){

    /** 
     * This part does the "fixed navigation after scroll" functionality
     * We use the jQuery function scroll() to recalculate our variables as the 
     * page is scrolled/
     */
    $(window).scroll(function(){
        var window_top = $(window).scrollTop() + 12; // the "12" should equal the margin-top value for nav.stick
        var div_top = $('#nav-anchor').offset().top;
            if (window_top > div_top) {
                $('nav').addClass('stick');
            } else {
                $('nav').removeClass('stick');
            }
    });

    /**
     * This part causes smooth scrolling using scrollto.js
     * We target all a tags inside the nav, and apply the scrollto.js to it.
     */
    $("nav a").click(function(evn){
        evn.preventDefault();
        $('html,body').scrollTo(this.hash, this.hash); 
    });

    /**
     * This part handles the highlighting functionality.
     * We use the scroll functionality again, some array creation and 
     * manipulation, class adding and class removing, and conditional testing
     */
    var aChildren = $("nav li").children(); // find the a children of the list items
    var aArray = []; // create the empty aArray
    for (var i=0; i < aChildren.length; i++) {    
        var aChild = aChildren[i];
        var ahref = $(aChild).attr('href');
        aArray.push(ahref);
    } // this for loop fills the aArray with attribute href values

    $(window).scroll(function(){
        var windowPos = $(window).scrollTop(); // get the offset of the window from the top of page
        var windowHeight = $(window).height(); // get the height of the window
        var docHeight = $(document).height();

        for (var i=0; i < aArray.length; i++) {
            var theID = aArray[i];
            var divPos = $(theID).offset().top; // get the offset of the div from the top of page
            var divHeight = $(theID).height(); // get the height of the div in question
            if (windowPos >= divPos && windowPos < (divPos + divHeight)) {
                $("a[href='" + theID + "']").addClass("nav-active");
            } else {
                $("a[href='" + theID + "']").removeClass("nav-active");
            }
        }

        if(windowPos + windowHeight == docHeight) {
            if (!$("nav li:last-child a").hasClass("nav-active")) {
                var navActiveCurrent = $(".nav-active").attr("href");
                $("a[href='" + navActiveCurrent + "']").removeClass("nav-active");
                $("nav li:last-child a").addClass("nav-active");
            }
        }
    });
});
```

## Wrap Up

Here, we see the versatility of the `scroll()` function from JQuery. The possibilities are endless by manipulating it, and this is a great example of using it. Don’t forget to check out the demo or download the source by clicking the links below, and feel free to leave a comment if you have any feedback or questions.

<p class="text-align--center">
<a href="http://callmenick.com/_development/single-page-smooth-scroll/single-page-smooth-scroll-soure.zip" class="button button--inline-block button--medium">Get Source</a>
<a href="http://callmenick.com/_development/single-page-smooth-scroll" class="button button--inline-block button--medium">View Demo</a>
</p>
<p class="text-align--center">
<a href="http://callmenick.com/lab-demos/14-tabbed-content/14-tabbed-content.zip" class="button button--inline-block button--medium">Get Source</a>
<a href="http://callmenick.com/lab-demos/14-tabbed-content/" class="button button--inline-block button--medium">View Demo</a>
</p>

Our tabbed contend area is based on a simple user interaction. We have some tabs that act as a navigation, and some content areas (`sections`) that are controlled by these tabs. When one tab is selected, it is highlighted and the corresponding `section` is shown, while the rest of the sections are hidden.

## Getting Started

The structure of our tabbed content area will be simple. We’ll have one unordered list that acts as the tabs, and another unordered list that acts as the sections. Our tabs list will control which section is being shown, by using a bit of jQuery magic and the CSS property display. The active section will also be indicated by the corresponding tab being highlighted. Let’s take a look at the HTML markup.

```html
<ul id="tabs">
    <li class="active">Tab 1</li>
    <li>Tab 2</li>
    <li>Tab 3</li>
    <li>Tab 4</li>
    <li>Tab 5</li>
</ul>
<ul id="tab">
    <li class="active">
        <h2>This is the first tab</h2>
    </li>
    <li>
        <h2>This is the second tab</h2>
    </li>
    <li>
        <h2>Tab number three wee hee</h2>
    </li>
    <li>
        <h2>Fourth tab not bad</h2>
    </li>
    <li>
        <h2>Tab number five here we go!</h2>
    </li>
</ul>
```

## Some Notes About The HTML

Here, we see a basic structure for two unordered lists. The `#tabs` list acts as the navigation control, and the `#tab` list acts as the section control. We notice that the first li  element in both lists has a class of active. This is to initialise our tabbed area, as the rest of the sections will be hidden, and the rest of the tabs will be inactive. Let’s now take a look at the CSS, which will style our tabs nicely, and hide our section contents that are inactive. Without further ado, the CSS!

```css
ul#tabs {
    list-style-type: none;
    padding: 0;
    text-align: center;
}
ul#tabs li {
    display: inline-block;
    background-color: #32c896;
    border-bottom: solid 5px #238b68;
    padding: 5px 20px;
    margin-bottom: 4px;
    color: #fff;
    cursor: pointer;
}
ul#tabs li:hover {
    background-color: #238b68;
}
ul#tabs li.active {
    background-color: #238b68;
}
ul#tab {
    list-style-type: none;
    margin: 0;
    padding: 0;
}
ul#tab li {
    display: none;
}
ul#tab li.active {
    display: block;
}
```

## Some Notes About the CSS

We style our tabs to be simple inline-style tabs using the CSS property `inline-block`. We also give the active tab its own style, so we can see which tab is currently being displayed. We then style our tabbed content area as block-style elements by using `display:block`, but we hide all of them initially using `display:none`. Now, time to make it all work. Here’s the jQuery that’ll handle it.

```javascript
$(document).ready(function(){
    $("ul#tabs li").click(function(e){
        if (!$(this).hasClass("active")) {
            var tabNum = $(this).index();
            var nthChild = tabNum+1;
            $("ul#tabs li.active").removeClass("active");
            $(this).addClass("active");
            $("ul#tab li.active").removeClass("active");
            $("ul#tab li:nth-child("+nthChild+")").addClass("active");
        }
    });
});
```

## Explaining the JavaScript/jQuery

Our simple jQuery function above lets us show and hide the correct content area when a tab is clicked. When you click a tab, it fires the `.click()` function. It checks to see if the current tab has a class of active. If it doesn’t, then we proceed to the logic. It checks which tab index is clicked using the `.index()` function. DOM indexes start from 0, so we have to add 1 to find the correct corresponding nth-child. We then remove the `active` class from the current tab, and add it to the tab we just clicked. We then find the content area that’s currently being displayed and hide it by removing the `active` class from it. Finally, we add the `active` class to the nth-child content area that we found just above.

## Wrap Up

And there you have it! A simple tabbed content area, using CSS and jQuery. It’s highly customizable, and you can easily drop it into any of your projects. Thanks for checking it out, and feel free to download the source code or leave any comments/questions below.

<p class="text-align--center">
<a href="http://callmenick.com/lab-demos/14-tabbed-content/14-tabbed-content.zip" class="button button--inline-block button--medium">Get Source</a>
<a href="http://callmenick.com/lab-demos/14-tabbed-content/" class="button button--inline-block button--medium">View Demo</a>
</p>
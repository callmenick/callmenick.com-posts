In this lab, we create a single page FAQ with toggling answers using JQuery and some CSS. This is a good solution because it conserves space, allowing users to easily see all the FAQ’s first before clicking on one to see the answer. Clicking on one also toggles or slides the answer down into view, instead of moving to another page. Without further ado, let’s get into it.

## Marking Up The HTML

First, let’s markup the basic HTML. Our HTML will consist of some `h2` tags that have the FAQ questions, and under each `h2` tag, a `div` tag that will contain the FAQ answers. With that in mind, one question should look something like this.

```language-markup
<section id="faq-list">
    <h2>Is it true that you are a carrot?</h2>
    <div class="answer">
        <p>Lorem ipsum dolor sit amet....</p>
    </div>
</section>
```

## Formatting With CSS

Now, let’s look at the CSS. The first thing we need to do is hide the answers. This is done by adding `display:none;` to the `.answer` class. Next, we want people to know that our `h2` tags are clickable and expandable. We do this by adding a background image (I use a plus sign image that I made in Adobe Illustrator) to the `h2` tag. Next up, we need to think ahead. When the answer slides down, we want this image to be a minus sign, so people know it’s expanded and can be clicked to be hidden. We create a new `h2` class called `h2.close` and give it our new background image. Our CSS should now look something like this.

```language-css
.answer {
    display:none;
}

h2 {
    line-height:24px;
    font-size:18px;
    font-weight:700;
    color:rgb(100,150,200);
    padding-left:24px;
    cursor:pointer;
    background-image:url('../img/open.png');
    background-position:left;
    background-repeat:no-repeat;
}

h2.close {
    background-image:url('../img/close.png');
}
```

## Making It Happen With JQuery

Make sure you include the latest version of JQuery in your file, otherwise this won’t work. After you’ve done that, let’s look at what we need to do. We want to target an `h2` element, and when it is clicked, we want the subsequent `div` with class `.answer` to toggle down. We also want the background image of the `h2` tag to change. Wrapping all our answers in a `div` with id `#faq-list`, we can target the `h2` tags inside this div. After that, we add a `.click()` function to these `h2` tags. Then, we use the JQuery `$(this)` to target the current `h2` tag in use. Using `.next()`, we can find the next tag that has the class `.answer`, and apply a `.slideToggle()` to it. We can also use `.toggleClass()` on the current `h2` tag in effect, to switch the background. If you want to read up more on any of these JQuery functions, check out the JQuery API documentation. With all that in mind, our JQuery should look something like this.

```language-javascript
$(document).ready(function() {
    $('#faq-list h2').click(function() {

        $(this).next('.answer').slideToggle(500);
        $(this).toggleClass('close');

    });
}); // end ready
```

Thanks for reading! Don’t forget, you can download the source or view the demo by clicking on the links below. Don’t forget to leave a comment!
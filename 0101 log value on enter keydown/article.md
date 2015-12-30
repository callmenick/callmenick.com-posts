## Introduction

A couple weeks ago, a friend of mine asked for a bit of help with a small problem. He wanted to capture the value of a text input field every time a user typed something and hit enter. With modern-day frameworks like Angular and React at our disposal, this is a breeze because of built in data-binding. Of course, though, everyone and every solution doesn't make use of these frameworks. Luckily, the code is simple enough. Below are two snippets - one with plain ol' JavaScript, and the other with jQuery. Both are based on targeting a generic text field:

```html
<input type="text">
```

## The Snippet

The snippet is basically saying:

> listen for a `keydown` event on the text input, and if that key is the enter key (i.e. a `keyCode` of 13), log the value of that text input.

Here it is in JavaScript:

```javascript
(function() {
  document.querySelector('input').addEventListener('keydown', function(e) {
    if (e.keyCode === 13) {
      // do whatever you want with the value
      console.log(this.value);
    }
  });
})();
```

And again, with jQuery:

```javascript
$(document).ready(function() {
  $('input').keydown(function(e) {
    if(e.keyCode === 13) {
      // do whatever you want with the value
      console.log($(this).val());
    }
  });
});
```

## Wrap Up

And that’s a wrap! I hope this snippet is useful to you in your future endeavors. Thanks for reading, and feel free to leave any questions, comments, or feedback below.
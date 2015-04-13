get-position-featured-image

## The X-Y Position Function

Getting the x and y coordinates of a user’s mouse relative to the document can seem tricky. Different browsers have different properties at their disposal to calculate mouse position. Here’s a cross browser solution for finding the x and y coordinates.

```javascript
function getPosition(e) {
  var posx = 0;
  var posy = 0;

  if (!e) var e = window.event;
  
  if (e.pageX || e.pageY) {
    posx = e.pageX;
    posy = e.pageY;
  }
  else if (e.clientX || e.clientY) {
    posx = e.clientX + document.body.scrollLeft
      + document.documentElement.scrollLeft;
    posy = e.clientY + document.body.scrollTop
      + document.documentElement.scrollTop;
  }

  return {
    x: posx,
    y: posy
  }
}
```

## Usage

Using the function is easy now. We just need to pass in the event handler when we’re calling our function. If we wanted to get the x and y coordinates of where a user has clicked on the document, we’d do it like this:

```javascript
document.addEventListener( "click", function(e) {
  var x = getPosition(e).x;
  var y = getPosition(e).y;
  console.log("x pos: "+ x +" // y pos:"+ y);
});
```

## Wrap Up

This cross-browser solution is very handy, and many different events can be passed in that will allow us to get the x and y position on the fly. I hope you find this snippet useful! If you have it in action or if you have and questions, comments, or feedback, write in the comment box below.

## Resources

* Quirks mode: [http://www.quirksmode.org/js/events_properties.html](http://www.quirksmode.org/js/events_properties.html)
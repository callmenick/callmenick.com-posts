## The Script

This neat little script will check if everything (and I mean everything) has fully loaded in our document. This script is very useful as sometimes, we might want to run a function only after everything has fully loaded. JavaScript at the end of our document will run before assets like images are loaded, because the DOM elements have all been loaded and that’s what we really need for DOM JavaScript to run. What if we wanted to hide our page until everything is fully loaded? Here’s the script:

```javascript
var everythingLoaded = setInterval(function() {
  if (/loaded|complete/.test(document.readyState)) {
    clearInterval(everythingLoaded);
    init(); // this is the function that gets called when everything is loaded
  }
}, 10);
```

## Breaking Down The Script

Let’s dig into the various parts of this script. We’re running an interval function first that will repeat itself every .01 seconds until our condition is met. At this point, we’ll clear the interval and it will no longer run. Inside our interval function, we’ll check for the `readyState` of the document. According to the MDN, `document.readyState` will:

> Returns “loading” while the document is loading, “interactive” once it is finished parsing but still loading sub-resources, and “complete” once it has loaded.

We want to check if we get “complete” returned, at which point we’ll know everything has loaded. I’ve read that some browsers return “loaded” instead of “complete” though, so using JavaScript’s `.test()` function, we’ll check for both “loaded” and “complete”. Once returned, the interval is cleared, and we call our `init()` function.

## Shortcomings…Beware!

I received some awesome feedback on Google+ from [Michael Ahlers](https://plus.google.com/u/0/+MichaelAhlers/posts) that gave me some fantastic insight. His first words to me took this form:

>…in reality, additional resources may be requested at any time. If you need to know, correctly, when your dependencies are loaded, you need to a library like RequireJS.

After some more communication, he gave me a real world example of where this method would fall short. Here’s what he said:

> Here’s a real world scenario. A developer’s using Google Maps API, which uses a bootstrap script to load additional libraries based on what’s required by the developer. The completion of that additional loading is non-deterministic. If they’re using your library, they may (or may not) get a false assurance all resources loaded and may attempt to start using the Maps API…The browser can only tell you when all resources immediately required by the page are loaded. Everything that happens after that is a total crapshoot.

This is where it all came together for me in my head, so much thanks to him, and I dug into more and more research. We simply can’t achieve this confidently with just a few lines of JavaScript. However, it does have its case. If you’re developing a simple application where all the JS is yours and yours alone, and you know that it has no external dependencies that might cause a delay in loading, then go ahead and use it. However, when you start to implement APIs and such, and additional resources are required within those APIs, you’ll run into trouble with this.

## Solutions?

The are full-on solutions out there for larger scale apps, a popular one being [Require.js](http://requirejs.org/) (as mentioned above). Require.js is a JavaScript file and module loader, and is the all in one solution when you need to load multiple scripts that are potentially loading external resources. It’s easy to set up, and there is some nice documentation on their site. Check it out!

## Wrap Up

And that’s a wrap! We’ve seen again here how JavaScript trick us a little and give us a false sense of belief, but there’s always a way, and developers out there are doing fantastic work to make the web a better place for us developers. If you’re developing a simple all though, then the script above is simple and elegant, ready to drop into any of your applications. Thanks for reading, and if you have any questions, comments, or feedback, feel free to leave them below.
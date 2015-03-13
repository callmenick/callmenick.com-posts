A lot of great JavaScript is heavily centred around adding and removing of classes. Nowadays, CSS3 transitions, transforms, and animations are starting to take the forefront, and so a lot of really cool effects can be achieved by simply adding a class to an element. Want an image to transition out of view on touch? Just add some transition properties to the image, and a related touch class that has the transform properties. Add the class to the image on a touch event via JavaScript, and voila, smooth, beautiful, CSS3 transitions.

## Classie.js is Awesome

Adding and removing classes without a library such as jQuery isn’t the easiest of things, and sometimes JS libraries are just too bloated for your needs. In a lot of my projects where I’ve used libraries, I find myself only ever requiring class toggling/checking functionalities. Enter [classie.js](https://github.com/desandro/classie/blob/master/classie.js), an awesome mega lightweight script that allows you to add, remove, toggle, and check for classes in the DOM very easily. [Classie.js](https://github.com/desandro/classie/blob/master/classie.js) gives us fantastic and simple usability, without bloating our scripts at all. At a mere 82 lines of code, it comes in at just 1.872 kb. Not bad. Here’s a simple example of classie.js in action:

```javascript
var el = document.getElementById("header");
if (classie.has(el,"sweet")) {
	classie.remove(el,"sweet");
} else {
	classie.add(el,"sweet");
}
```

In the above example, `el` represents our element in query. We’re checking if it has a class of `sweet`. If it does, we remove it, and if not, we add it. And there you go, classie.js in action! With JavaScript’s latest updates, it seems as if vanilla JS is staking its claim once again in the developer world. Little scripts like this are definitely the way forward right now in a world of bloated libraries.

<p class="text-align--center">
<a href="https://github.com/desandro/classie/blob/master/classie.js" class="button button--inline-block button--medium">Stay Classy With Classie.js</a>
</p>
Out with the old, in with the new. We’re in a day and age where we need to simplify our stylesheets and lighten our page loads to be efficient across all devices on all connections. There’s a balance between simplicity and beauty though. We of course want to be functional and progressive, but also want to be aesthetically pleasing. There was once a time when graded images with shadows, hover states, and active states were used as buttons. It added some user-friendliness to our forms and whatnot. Nowadays, with CSS3 in the fold and a more compact, minimal style taking precedence, we can achieve stylish CSS buttons. The reason for switching to pure CSS is twofold -

1. It’s a lighter load and easier to change up
2. They become easier to expand/shrink depending on screen size, by simply changing values in your media queries

In this tutorial, I’m going to run through the basics of 4 different style buttons – flat, border, gradient & shadow (skeumorphic), and press style buttons. I’ll reset all the CSS so that buttons display consistently across devices, and I’ll style each set of buttons with a `:hover` and `:active` state. Without further ado, let’s dig into the HTML.

## Getting Started with the HTML

The HTML I’m using is straightforward. I have four sections, each with their own class, that will allow me to target the buttons inside that section. Of course, if you want to target all or specific buttons on your site, then adjust accordingly by adding classes to specific buttons, or not adding classes at all. I also added `.hover` and `.active` classes to some of the buttons, so that you can see what they look like in all three states in the demo without actually hovering and pressing. Here’s my HTML set up:

```language-markup
<section class="flat">
    <button>Button</button>
    <button class="hover">:hover</button>
    <button class="active">:active</button>
</section>
<section class="border">
    <button>Button</button>
    <button class="hover">:hover</button>
    <button class="active">:active</button>
</section>
<section class="gradient">
    <button>Button</button>
    <button class="hover">:hover</button>
    <button class="active">:active</button>
</section>
<section class="press">
    <button>Button</button>
    <button class="hover">:hover</button>
    <button class="active">:active</button>
</section>
```

## The CSS For All Buttons

Different browsers handle default styling of buttons differently. The following CSS resets all the styles, and adds some defaults (that you can adjust). We’re going to get rid of any default shadows, border-radiuses, appearances, and outlines. Here’s what the CSS looks like:

```language-css
button {
  display: inline-block;
  margin: 0 10px 0 0;
  padding: 15px 45px;
  font-size: 48px;
  font-family: "Bitter",serif;
  line-height: 1.8;
  appearance: none;
  box-shadow: none;
  border-radius: 0;
}
button:focus {
  outline: none
}
```

Easy enough. Now let’s dig into each of our button styles. Note: I didn’t prefix any of my CSS properties for the sake of simplicity. To ensure as much cross browser uniformness as possible, prefix your necessary styles accordingly.

## 1) Flat Style CSS Buttons

Flat style buttons are very popular these days, and directly in line with the flat-design trends that are everywhere. Buttons in the past tended to stand out a bit more, but people are accustomed to flat buttons these days, and these style buttons work just fine. Just make sure you line them up in areas where users would expect to find a button. Here’s an image of this CSS button in its normal, hover, and active states.

<img class="aligncenter size-full wp-image-528" alt="flat" src="http://www.callmenick.com/wp-content/uploads/2014/04/flat1.png" width="760" height="123">

The CSS for this is fairly straightforward. We get rid of the border, add a background colour and a text shadow. The hover and active states are indicated by a slightly darker background colour and text shadow. Here’s the CSS:

```language-css
section.flat button {
  color: #fff;
  background-color: #6496c8;
  text-shadow: -1px 1px #417cb8;
  border: none;
}
section.flat button:hover,
section.flat button.hover {
  background-color: #346392;
  text-shadow: -1px 1px #27496d;
}
section.flat button:active,
section.flat button.active {
  background-color: #27496d;
  text-shadow: -1px 1px #193047;
}
```

## 2) Border Style CSS Buttons

Border style buttons are in the same class as flat buttons. The only difference is that instead of using a background colour, we’ll use a border instead. Here’s an image of this CSS button in its normal, hover, and active states.

<img class="aligncenter size-full wp-image-529" alt="border" src="http://www.callmenick.com/wp-content/uploads/2014/04/border.png" width="802" height="146">

Once again, the CSS is fairly straightforward, and we’ll just be targeting the border colour and text colour. Each of these two properties gets a bit darker on hover and active. Here’s the CSS:

```language-css
section.border button {
  color: #6496c8;
  background: rgba(0,0,0,0);
  border: solid 5px #6496c8;
}
section.border button:hover,
section.border button.hover {
  border-color: #346392;
  color: #346392;
}
section.border button:active,
section.border button.active {
  border-color: #27496d;
  color: #27496d;
}
```

## 3) Gradient & Shadow Style CSS Buttons (Skeumorphic)

These buttons are more in line with what we’re accustomed to seeing in the days gone by. If those multicoloured gradient/shadow style buttons are suited to your project’s style, then these are for you. The best thing is that it’s all done with CSS, so can easily be scaled up or down without resolution loss, or having to create new images. Here’s an image of this CSS button in its normal, hover, and active states.

<img class="aligncenter size-full wp-image-530" alt="gradient" src="http://www.callmenick.com/wp-content/uploads/2014/04/gradient.png" width="764" height="144">

The CSS for this style makes use of gradients and shadows. On hover, a shadow will appear around the button, and on active, the shadow will be inset to give it a “pressed” look. Flat background colours will serve as the fallback for browsers that lack gradient support, and the shadow of course is just embellishment. Here’s the CSS:

```language-css
section.gradient button {
  color: #fff;
  text-shadow: -2px 2px #346392;
  background-color: #ff9664;
  background-image: linear-gradient(top, #6496c8, #346392);
  box-shadow: inset 0 0 0 1px #27496d;
  border: none;
  border-radius: 15px;
}
section.gradient button:hover,
section.gradient button.hover {
  box-shadow: inset 0 0 0 1px #27496d,0 5px 15px #193047;
}
section.gradient button:active,
section.gradient button.active {
  box-shadow: inset 0 0 0 1px #27496d,inset 0 5px 30px #193047;
}
```

## 4) Press Style CSS Buttons

These “press” style buttons combine a bit of flat design and skeumorphism, giving the user the feeling that they actually pressed the button. It seemingly sinks into the page when the user pressed it. Here’s an image of this CSS button in its normal, hover, and active states.

<img class="aligncenter size-full wp-image-531" alt="press" src="http://www.callmenick.com/wp-content/uploads/2014/04/press.png" width="760" height="135">

The CSS here is slightly more complicated than the previous demos, and requires a little bit of math. But it’s still very easy to grasp. We style our button initially with an un-blurred box shadow to give it a 3D pop out look. On hover, the background colour gets a bit darker. When the button is pressed, the shadow gets smaller, and we will apply a CSS3 transform (translateY) downward equal to the difference in the box shadow normal and active values. Sounds a lot more complicated than it is, but take a look at the CSS and you’ll have a clearer understanding. Here it is:

```language-css
section.press button {
  color: #fff;
  background-color: #6496c8;
  border: none;
  border-radius: 15px;
  box-shadow: 0 10px #27496d;
}
section.press button:hover,
section.press button.hover {
  background-color: #417cb8
}
section.press button:active,
section.press button.active {
  background-color: #417cb8;
  box-shadow: 0 5px #27496d;
  transform: translateY(5px);
}
```

## Wrap Up

And there you go! Stylish, elegant, and funky CSS buttons for all your needs. They are highly customisable, scalable, and easy to use. There are some basic CSS media queries in the source folder, but you might want to use your own sizing/media queries in your project. Don’t forget, you can download the source or view the demo by clicking the links below. Thanks for reading, and feel free to leave any comments, suggestions, or opinions below!
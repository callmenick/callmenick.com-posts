## Why Use Icons?

Icons give us an opportunity to break the monotony in our design patterns by adding a little extra finishing touch to our user interface. They can neatly break up linear sections of text, give  headlines a little punch, enhance "call to action" elements, and a whole lot more. They've also become standardised over time, and users have an increased recognition for icons, associating them with certain things. A phone icon, for example, is a timeless representation of contact information. A newer one would be a map pin, signifying a map, directions, or address of some sort.

Icons add more appeal, and there's no reason to not include them in your design today. CSS and browsers makes it even easier these days for us with near-total support for pseudo elements, in particular, the `::before` and `::after` elements. These pseudo elements allow us to truly separate content from style, while still enhancing the user experience. We no longer need extra markup, empty `span`'s, and the likes, to achieve this additional detail in your design. The implementation for front end developers is now so simple! Let's look at some code snippets and visual examples CSS icons in action.

## CSS Icons In Action

Let's take a look at some CSS icons in action. My very own site utilizes icons in the header and on demo pages.These icons tell the user that they can visit various social media platforms that I'm on, sign up for a newsletter, or perform a search. Instead of boring text links to these various parts, these icons appear in a very welcoming manner, adding a little warmth and vibrancy to the header. Here's what the header looks like:

IMG

In the above case, the icons represent links to various parts. They stand on their own, add a little colour and vibrancy to the header, and immediately let the user know what is what. The icons are all recognizable, and this encourages users to click on them. Another scenario where CSS icons boost the appearance of your site would be when the precede a text link. A classic example would be a download button. Instead of it just saying download, it can have a little recognizable download icon before the string of text. I use this variation on my site too on tutorial pages, flavouring up my "get source" and "view demo" links. The icons are added via pseudo classes, hence not interfering with the semantic markup of my site. Here's what they look like:

IMG

The code to achieve this looks a bit like this:

```css
a.nice-button {
  display: inline-block;
  margin: 0 5px 5px 5px;
  background-color: rgb(40,170,220);
  padding: 20px;
  color: #fff;
  font-size: 14px;
  text-align: center;
}

a.nice-button:hover {
  background-color: rgb(10,140,190);
  color: #fff;
}

a.get-source:before,
a.view-demo:before {
  display: block;
  margin-right: 10px;
  float: left;
  width: 20px;
  height: 20px;
  content: "";
}

a.get-source:before {
  background: url("img/download.png");
  background: url("img/download.svg"), none;
}

a.view-demo:before {
  background: url("img/eye.png");
  background: url("img/eye.svg"), none;
}
```

Using the pseudo classes `::before` and `::after`, I'm able to insert icons directly in the CSS without adding any extra non-semantic markup. Separation of content from style - all the cool kids are doing it, and you should too.

## Progressive Enhancement

The good thing about using icons in pseudo classes is that if a browser doesn't support the pseudo classes, it falls back gracefully, just omitting the icon entirely. Because you are separating content from style (you are, right?), this should be no problem at all for you. In the case mentioned above, the links will just show up as text buttons saying "get source" and "view demo", without displaying the icons at all. Not bad at all. In case 1, where we looked at icons in the header, browser compatibility should be at a maximum, as we're just using them as background images to our elements.

Another progressive enhancement thought is thinking about serving different flavours as screen size gets bigger. For example, at a small screen size, you might want to consider minimizing the real estate that some of your secondary information takes up. On bigger screens, you'd want to give the user a bit more punch and detail. With pseudo classes in action, it's easy, because you can show/hide icons or those not-totally-important parts of your content with media queries. Here's a visual example:

IMG

## SVG &amp; Icon Fonts To The Rescue

There are two great ways to implement icons in your design, allowing for scalability and retina-readiness without any crazy efforts. They are SVG's and Icon Fonts. Both have their pluses and minuses.

### SVG

SVG icons are great, because you get to define them at a size you want, and they'll render at exactly that size. You can implement them as background images in elements or pseudo elements, support is high, and a graceful fallback is easy. They are also very lightweight, adding very little overhead to your project. And of course, they are retina ready. The control you have over them with CSS is a bit limited though, and you'll find yourself still having to make image sprites if you want certain changes on hover or active. I still use SVG icons personally, because of how lightweight and easy it is.

### Font Icons

Font icons also have their plus side. A font icon is ultimately...well...a font, so you can achieve all the normal font properties you wish in your CSS. Font sizes, line heights, colours, text-shadows, and the works. It's worth mentioning too that `@font-face` support is very high, and unless you find yourself gasping for breath and perfection on IE7 and down (oh, the horror!), you'll be just fine implementing them right away. My issue with font icons though is that they don't always behave exactly as you want. Different browsers tend to render fonts differently, and those sizes or line-heights may not always look the same. Also, in the past, I've often found myself using these bloated font-icon libraries adding a heap of overhead to my projects (which is why I stuck with SVG). I know you can create your own icon font-face and include only what you need, but I haven't found any real reasons to go back to font icons.

## Don't Overkill

With all these beautiful icons floating around the place, it's easy to get carried away. All of a sudden, everything looks better with an icon in front of or behind it! Please though, remember that icons serve a purpose, and that purpose is to visually enhance your project WITHOUT obstructing the most important parts - the content. Don't add icons to every heading tag in your project. Instead, add them to only `h1`'s. The last thing you want to do is clutter the space and create a bad experience for users.

## Wrap Up

That's a wrap! In this article, we looked at how icons can visually enhance your projects, while keeping some separation between content and style. We also looked at icons acting as visual links to familiar places, as opposed to just plain ol' text links. And finally, we looked at two options for implementing them, both with their share of plus sides. Are you implementing icons in your project? Share any questions, comments, and feedback below, and thanks for reading.
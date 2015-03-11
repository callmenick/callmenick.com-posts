If you’re not using SVG in your projects now, you should. SVG is awesome, and it’s a great way to minimise and simplify your image sprites, particularly when it comes to things like icons. If you have any vector graphics saved as PNG’s at all on your site actually, you should be jumping on board the SVG train. The following snippets showcase three ways to implement SVG’s, and have the SVG fallback to a PNG when SVG isn’t supported in the browser. Here’s what we’ll cover:

* SVG used as a background image, fallback with Modernizr
* SVG used as a background image, fallback with pure CSS
* SVG used in an inline img tag, with an onerror fallback
* SVG used in an inline img tag, with a Modernizr fallback

[Modernizr](http://modernizr.com/) is an awesome browser feature detection script, and will plug some classes into your opening html element in your document depending on whether a feature is detected or not. In this case, it will plug either the `svg` or `no-svg` class into the opening html element, depending on whether the browser supports or doesn’t support SVG. Let’s get started.

## 1) SVG Used as a Background Image, PNG Fallback with Modernizr

Using Modernizr, we can detect whether the browser supports SVG or not. If we’re using our SVG as a background image, then it’s as simple as saving the SVG and PNG versions of the image, and writing one extra CSS class declaration. This won’t have any double download issues, because the no-svg class would never be seen if SVG is supported, and vice versa. Here’s the CSS:

```language-css
.tomato {
	background-image: url('img/tomato.svg');
}
.no-svg .tomato {
	background-image: url('img/tomato.png');
}
```

## 2) SVG Uses as a Background Image, PNG Fallback with Pure CSS

This technique is even a bit cooler, because it doesn’t depend on any libraries. If you’ve already included Modernizr in your project, then it doesn’t matter too much. But if you’re considering including Modernizr in your project just for the sake of having a fallback for a background SVG image, then you’ll be better off using this method. This neat CSS trick works because multiple background image support is pretty much the same as SVG support, so if multiple backgrounds isn’t supported, the browser will fallback to the first declaration, hence displaying the PNG version.

```language-css
.tomato {
	background: url('img/tomato.png');
	background-image: url('img/tomato.svg'), none;
}
```

## 3) SVG Used in an Inline Image Tag, with an On Error Fallback to PNG

This method is good if you have access to your HTML tag yourself. It uses the `onerror`  function inside the `img` tag itself, which allows us to provide a fallback if the source image can’t load. Be careful though…if the fallback isn’t found, some browsers might go into an endless loop. Bad.

```language-markup
<img src="tomato.svg" onerror="this.src='tomato.png'">
```

It could become tricky if you have to upload and change multiple SVG’s without direct access to the source code though. Practicality issues may arise, and if that’s the case, you can proceed to #4 below, which will auto swap the SVGs with PNGs using JavaScript. The endless loop can be avoided thanks to this little trick, __provided by Roy Reed in the comments section:__

```language-markup
<img src="tomato.svg" onerror="this.src='tomato.png'; this.onerror=null;">
```

## 4) SVG Used in an Inline Image Tag, with Modernizr JavaScript Fallback to PNG

This method does however require Modernizr for its feature detection. Basically, we’ll detect if SVG is supported in the user’s browser. If it is, then we’ll leave our inline `img` tag as is. If not, we’ll run a little script that searches for `img` tags that point to SVG files, and replace the .SVG extensions with .PNG ones. You’ll need to make sure you have both  SVG and PNG versions of your file present, and with the same name, otherwise nothing will load. Here’s the HTML and JavaScript. Make sure Modernizr is included.

```language-markup
<img src="tomato.svg">
```

```language-javascript
if (!Modernizr.svg) {
    var imgs = document.getElementsByTagName('img');
    var svgExtension = /.*\.svg$/
    var l = imgs.length;
    for(var i = 0; i < l; i++) {
        if(imgs[i].src.match(svgExtension)) {
            imgs[i].src = imgs[i].src.slice(0, -3) + 'png';
            console.log(imgs[i].src);
        }
    }
}
```

## Wrap Up

I must reiterate that if you’re not currently using SVG in your projects, you should. Support is already high, and soon enough it will be supported by the extreme majority of users. There are many advantages of using SVG in your projects too. For starters, you don’t have to worry about retina display versions of your PNGs. SVGs are resolution independent, meaning responsive design is a breeze. SVG images can also be created directly in your HTML document, without even having to save the file itself. If you right click on your SVG file and open it in a text editor, you’ll see the code for your SVG. The possibilities are great with SVG, so jump on board the SVG train and don’t be afraid to use them! They’re friendly and pretty.
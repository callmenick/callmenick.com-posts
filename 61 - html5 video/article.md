<p class="text-align--center">
<a href="http://callmenick.com/tutorial-demos/html5-video/html5-video-source.zip" class="button button--inline-block button--medium">Get Source</a>
<a href="http://callmenick.com/tutorial-demos/html5-video" class="button button--inline-block button--medium">View Demo</a>
</p>


## Video In The Browser

For a long time, video in the browser never worked in a native manner. We always depended on external or non-native implementations. Most commonly, we used either embedded videos from 3rd party plugins (like Flash or QuickTime), or an `iframe` with a link to an externally hosted video (likely on YouTube or Vimeo). This may have served our basic needs for a long time, but manipulation, customisation, and seamless native implementation has been non-existent. Not to mention, third party plugins may have crashed (grey sad-face lego block anyone?) or not been enabled on some devices/browsers, serving up poor user experiences. When the HTML5 spec was released, it came with the `video` element, and with it came a whole new host of opportunities to be creative with video implementation. Oh, and responsive video too which is awesome. Let's take a look at the HTML5 `video` element, and get into some examples.

## The HTML5 Video Element

According to the MDN:

> The HTML `<video>` element is used to embed video content. It may contain several video sources, represented using the `src` attribute or the `<source>` element; the browser will choose the most suitable one.

Easy enough, right? Well, not so fast. Video itself is a long and complicated matter, with multiple formats around. A video isn't just a file. It's a container that stores different audio and video "tracks", and the container itself determines how these different tracks are fed back to the end user. There's a ton of info out there about this, but I won't bore you with details. We just need to know what works on the web. In general, we're looking at 3 different formats to cover all bases on the web:

* MP4 - a container for H.264 video (a very common video compression format) and AAC audio (lossy Advanced Audio Coding)
* Ogg - a container for Theora video (a lossy video layer) and Vorbis audio
* WebM - a container intended primarily for use in the HTML5 `video` tag, initially consisting of VP8 video and Vorbis audio, and more recently VP9 video and Opus audio

The above sounds a bit technical, but basically, you're just looking at files with the extensions `.mp4`, `.ogv`, and `.webm`. According to W3, using Ogg/Theora/Vorbis and MP4/H.264/AAC seems to cover most cases, however Ogg/Theora/Vorbis is being replaced in favour of WebM these days. We'll include all 3 for full coverage, and you can see a full [table of support here](http://en.wikipedia.org/wiki/HTML5_video#Browser_support).

## The Video Element In Action

The video tag in its most basic implementation is as easy as any other HTML tag. We can leave some text in between the tags to prompt users in an unsupported environment that video isn't supported, and perhaps provide a download link. Here's what it looks like:

```html
<video src="media/demo.ogv">
  Your browser doesn't support HTML5 video. Download it <a href="#">here</a>.
</video>
```

This of course is including only one source file, which we know from above is not the case. Luckily, the `video` element recognises the `source` element when placed inside it. According to the MDN:

> The HTML `<source>` element is used to specify multiple media resources for `<picture>`, `<audio>` and `<video>` elements.

More on the `source` element [here](https://developer.mozilla.org/en-US/docs/Web/HTML/Element/source). In its basic implementation, we include the two attributes `src` and `type`, so to cover ourselves, we can markup our video like this:

```html
<video>
  <source src="media/demo.mp4" type="video/mp4">
  <source src="media/demo.ogv" type="video/ogg">
  <source src="media/demo.webm" type="video/webm">
  Your browser doesn't support HTML5 video. Here's a <a href="#">link</a> to download the video.
</video>
```

Take note of the `type` attribute which is the MIME type of the resource. This attribute also takes on an optional `codecs` parameter. I won't go into detail about this, but you can read about MIME types and associated codecs [here](http://dev.w3.org/html5/spec-preview/the-source-element.html), but with codecs included, your markup may now look something like this:

```html
<video>
  <source src="media/demo.mp4" type="video/mp4; codecs='avc1.42E01E, mp4a.40.2'">
  <source src="media/demo.ogv" type="video/ogg; codecs='theora, vorbis'">
  <source src="media/demo.webm" type="video/webm; codecs='vp8, vorbis'">
  Your browser doesn't support HTML5 video. Here's a <a href="#">link</a> to download the video.
</video>
```

Note that you might have to work out the type parameters yourself depending on how you encode your videos. Speaking of encoding videos, let's take a look at some options.

## Encoding & Preparing Video For The Web

Here's a list of tools for converting files to be web ready:

1. "ffmpeg2theora" is a converter to create Ogg Theora files. It runs very easily via the command line. Download it [here](http://v2v.cc/~j/ffmpeg2theora/).
2. "ffmpeg" is a very robust converter for everything else. It also runs via the command line, and has a wide range of options. Download it [here](http://www.ffmpeg.org/).

OK, now that we've gotten through some technical stuff, let's take a look at some actual examples. Before we continue though, it's important to know that aside from just the `video` and `source` tags we've seen so far, there's also a host of attributes at our disposal which you can see [here](https://developer.mozilla.org/en-US/docs/Web/HTML/Element/video#Attributes). Great, onward.

## Example 1 - Utilising The Attributes

Firstly, we need some CSS so make our video fit the container width. This is because I'm using some wide/tall video files for later examples. Here's the CSS:

```css
/* ============================================================
  DEMO 1 - EXPLORING ATTRIBUTES
============================================================ */
video#cmn-video-demo1 {
  width: 100%;
}
```

Next, let's add the browser default controls. This is as simple as putting the `controls` in the `video` tag. Here's the HTML:

```html
<video id="cmn-video-demo1" controls>
  <source src="media/demo.mp4" type="video/mp4">
  <source src="media/demo.ogv" type="video/ogg">
  <source src="media/demo.webm" type="video/webm">
  Your browser doesn't support HTML5 video. Here's a <a href="#">link</a> to download the video.
</video>
```

If the video was the primary focus of our web page, and we wanted to engage the user immediately, how would we auto play it? Easy. Just use the `autoplay` attribute. Here's what our markup would look like now:

```html
<video id="cmn-video-demo1" controls autoplay>
  ...
</video>
```

And if it were just a video loop, or a video that you wanted to play back from the start when it reached the end? Easy again, as we have the `loop` attribute in our arsenal. Here's the markup again:

```html
<video id="cmn-video-demo1" controls autoplay loop>
  ...
</video>
```

Lastly, what if we wanted to display a different "poster" image for our video until the user played it, or it started playing? Guess what - there's a `poster` attribute for us. This attribute takes a url as its parameter, and this is what the markup would look like: 

```html
<video id="cmn-video-demo1" controls autoplay loop poster="media/poster.jpg">
  ...
</video>
```

Take note of the other attributes at our disposal too. For example, if you're using `autoplay`, you may want to consider muting the video as it is a bit intrusive to automatically bombard the user with sound. The `muted` attribute fills this purpose nicely. There's also the `width` and `height` attributes, both measured in CSS pixels. Now, let's add some custom controls.

## Example 2 - Video With Custom Controls

It's highly unlikely that the default controls provided by the various browsers matches your site aesthetic. That's OK though. There are a bunch of DOM methods available for us that will allow us to build a custom control structure and plug into our video. For our demo, we'll just build a play, pause, and mute controller. The play and pause control will be one button, and the text will change depending on whether it's playing or paused. The mute control will also change to "un-mute" if pressed. First of all, let's look at the markup:

```html
<video id="cmn-video-demo2">
  <source src="media/demo.mp4" type="video/mp4">
  <source src="media/demo.ogv" type="video/ogg">
  <source src="media/demo.webm" type="video/webm">
  Your browser doesn't support HTML5 video. Here's a <a href="#">link</a> to download the video.
</video>
<div id="cmn-video-demo2__controls">
  <button id="cmn-video-demo2__controls--play">Play</button>
  <button id="cmn-video-demo2__controls--mute">Mute</button>
</div>
```

Similar to last time, except now, we have some controls in action. Now, let's look at the CSS:

```css
/* ============================================================
  DEMO 2 - CUSTOM CONTROLS
============================================================ */
video#cmn-video-demo2 {
  width: 100%;
}

#cmn-video-demo2__controls button {
  display: inline-block;
  margin: 0;
  padding: 10px 20px;
  background-color: tomato;
  color: #fff;
  font-family: "Roboto Slab", serif;
  font-size: 12px;
  border: none;
  border-radius: 3px;
  box-shadow: none;
  appearance: none;
}
```

I put some resets on my buttons to match the demo aesthetic. Now, let's get into the fun parts. The DOM gives us the `.play()` and `.pause()` methods, and we know from before that `muted` is an attribute. With that in mind, building our control structure should be easy. We listen for clicks on our buttons, and apply the respective methods to our video. Here's the JavaScript:

```javascript
(function() {

  // cache vars
  var cmn_video = document.getElementById("cmn-video-demo2"),
      cmn_playback = document.getElementById("cmn-video-demo2__controls--playback"),
      cmn_mute = document.getElementById("cmn-video-demo2__controls--mute");

  // listen for playback
  cmn_playback.addEventListener( "click", function(e) {
    e.preventDefault();
    if (cmn_video.paused) {
      cmn_video.play();
      cmn_playback.innerHTML = "Pause";
    } else {
      cmn_video.pause();
      cmn_playback.innerHTML = "Play";
    }
  } );

  // listen for mute/unmute
  cmn_mute.addEventListener( "click", function(e) {
    e.preventDefault();
    if (cmn_video.muted) {
      cmn_video.muted = false;
      cmn_mute.innerHTML = "Mute";
    } else {
      cmn_video.muted = true;
      cmn_mute.innerHTML = "Unmute";
    }
  } );

})();
```

Now, let's look at some aesthetic implementations of HTML5 video.

## Example 3 - Video As A Background

I'm sure you've seen it implemented in some sites before. Maybe you've wondered how it was done. The truth is, it's simple. Background video isn't some big time CSS trickery. It's just a video in an absolutely positioned container, set behind some content. However, if we want to make our video "cover" the available background space, we need to leverage some tricks. I'm going to set three different `min-width` media query breakpoints, and force the video to be this width and centred. This will give the illusion that it's always covering the available space.

*Note: There is a lesser known CSS property, called `object-fit`. This property works in a very similar way to `background-size`, except we can apply it directly to elements as opposed to just backgrounds. Support, however, is a bit lacking. It appears in the CSS Images Level 4 spec though. There is a polyfill for it, but it's a bit buggy.*

Let's take a step by step look at what we want to achieve:

1. A banner-type div that is full-width, and has a video covering the entire width at all times, even when the screen is resized.
2. The video will be absolutely positioned so as to appear in the background.
3. We will also have a contents div above the video, with some fancy text and buttons.

And now, some code. Firstly, the markup:

```html
<div id="cmn-video-demo3__container">
  <video id="cmn-video-demo3__video" autoplay muted loop>
    <source src="media/demo.mp4" type="video/mp4">
    <source src="media/demo.ogv" type="video/ogg">
    <source src="media/demo.webm" type="video/webm">
    Your browser doesn't support HTML5 video. Here's a <a href="#">link</a> to download the video.
  </video>
  <div id="cmn-video-demo3__content">
    <h2>Kitty Kontrol</h2>
    <p>Learn to talk to kittens like a pro!</p>
    <a href="#">Get Started</a>
  </div>
</div>
```

It's worth noting that I muted the video, because I have `autoplay` set and the controls are hidden. I personally think this is a good idea, as sound can be intrusive, especially if the user has no way to turn it off. Now, let's look at the CSS:

```css
/* ============================================================
  BASE OVERRIDES

  Use this to make sure max width of wrapper is also max
  video width in demo 3.
============================================================ */
.wrapper.demo3 {
  max-width: 1600px;
}

/* ============================================================
  DEMO 3 - BACKGROUND VIDEO
============================================================ */
#cmn-video-demo3__container {
  position: relative;
  margin: 0 0 20px 0;
  height: 300px;
  background-color: #282828;
  overflow: hidden;
}

#cmn-video-demo3__video {
  position: absolute;
  top: 50%;
  left: 50%;
  z-index: 1;
  width: 600px;
  height: auto;
  transform: translate(-50%, -50%);
}

#cmn-video-demo3__content {
  position: absolute;
  top: 0;
  right: 0;
  bottom: 0;
  left: 0;
  z-index: 2;
  padding: 80px 20px;
  text-align: center;
}
#cmn-video-demo3__content h2,
#cmn-video-demo3__content p,
#cmn-video-demo3__content a {
  color: #fff;
  text-shadow: -1px 1px 4px rgba(0, 0, 0, 0.6);
}
#cmn-video-demo3__content h2 {
  font-family: "Roboto Slab", serif;
  font-size: 30px;
}
#cmn-video-demo3__content p {
  margin-bottom: 40px;
}
#cmn-video-demo3__content a {
  display: inline-block;
  margin: 0;
  padding: 10px 20px;
  font-family: "Roboto Slab", serif;
  border: solid 1px #fff;
  transition: background 0.3s, color 0.3s, text-shadow 0.3s;
}
#cmn-video-demo3__content a:hover {
  background-color: #fff;
  color: #787878;
  text-shadow: none;
}

@media all and (min-width: 600px) {
  #cmn-video-demo3__container {
    height: 480px;
  }

  #cmn-video-demo3__video {
    width: 1024px;
  }

  #cmn-video-demo3__content {
    padding: 160px 20px;
  }
  #cmn-video-demo3__content h2 {
    font-size: 42px;
  }
  #cmn-video-demo3__content p,
  #cmn-video-demo3__content a {
    font-size: 18px;
  }
}
@media all and (min-width: 1024px) {
  #cmn-video-demo3__container {
    height: 600px;
  }

  #cmn-video-demo3__video {
    width: 1600px;
  }

  #cmn-video-demo3__content {
    padding: 200px 20px;
  }
  #cmn-video-demo3__content h2 {
    font-size: 54px;
  }
  #cmn-video-demo3__content p,
  #cmn-video-demo3__content a {
    font-size: 24px;
  }
}
```

Take note that I added some custom styles for aesthetic purposes, and also some media queries for the sake of responsiveness. I recommend you change up these styles, fonts, colours, etc to match up with your own design, but if you like the demo as is, then feel free to use it! Also worth mentioning is the fact that the CSS below is un-prefixed, so prefix where necessary. All vendor prefixes are included in the downloadable source code.

Also, you might be wondering about support for CSS transforms. Luckily, it's pretty high, and the exact same as support for video (IE9 and up). So for full support, bring in a conditional stylesheet, reset the `top` and `left` values, and you're all set.

## Example 4 - Full Screen Background Video

Our final example will be to implement a fullscreen background video. Luckily for us, this video is self contained, and doesn't depend on widths or heights of any other containers in our markup. That makes it pretty easy to implement. Let's take a look at the markup:

```html
<main>  
  <div class="container">
    <div id="cmn-video-demo4__content">
      <div class="cmn-video-demo4__content--header">
        <h2>Kitty Kontrol</h2>
        <p>Learn to talk to kittens like a pro!</p>
        <a href="#">Get Started</a>
      </div>
    </div>
    <div class="cmn-video-demo4__content--main">
      <h2>About Kitty Kontrol</h2>
      <p>Lorem...</p>
    </div>
  </div>
</main><!-- /main -->

<video id="cmn-video-demo4__video" autoplay muted loop>
  <source src="media/demo.mp4" type="video/mp4">
  <source src="media/demo.ogv" type="video/ogg">
  <source src="media/demo.webm" type="video/webm">
  Your browser doesn't support HTML5 video. Here's a <a href="#">link</a> to download the video.
</video>
```

Because this video will have a `position: fixed` applied to it, it's important it's not nested inside any relatively positioned container. We want this video to occupy the full width and height of the screen, so it needs to position itself according to the window. Now, let's take a look at the CSS:

```css
/* ============================================================
  DEMO 4 - FULL SCREEN BACKGROUND VIDEO
============================================================ */
#cmn-video-demo4__video {
  position: fixed;
  top: 50%;
  left: 50%;
  z-index: -1;
  min-width: 100%;
  min-height: 100%;
  width: auto;
  height: auto;
  transform: translate(-50%, -50%);
}
```

I used the translate technique again to centre the video perfectly. I didn't include the other styles above because they are trivial in terms of the video aesthetic, but if you view the demo, you can see it in action. The source code also has all the CSS used, so download and use at your will. I repeated a lot of the CSS from example 3, even though repeating CSS is bad. So refactor as you see fit please! The one downside to this technique is that the video needs to be sized properly. At small screen widths, the clarity in the video will be lost. But then again, if you're using full screen video, I imagine you're targeting big screen users for full effect.

## Final Notes

As a final round up, here's some advice I have for you:

* For cross browser implementation, always provide a download link or a flash video as a backup video file
* Be sure to implement a custom IE8 and down stylesheet to reset styles for IE8
* Because video files tend to be big and bandwidths in general vary, I recommend always including a `poster` for your video, so users will see this until the video starts playing (if set to autoplay) or until they press play
* Make sure you have the right video files, MIME types, and codecs to make your videos suitable for the web

## Wrap Up

And that’s a wrap! It all seems like a lot to take in, but HTML5 video is pretty interesting and cool. We've looked at a few different things today, from default settings to custom controls to CSS aesthetic implementations. There's nothing stopping you from implementing HTML5 video in your next project! Don’t forget, you can view the demo and download the source by clicking the links below, and if you have and questions, comments, or feedback, you can also leave them below.

<p class="text-align--center">
<a href="http://callmenick.com/tutorial-demos/html5-video/html5-video-source.zip" class="button button--inline-block button--medium">Get Source</a>
<a href="http://callmenick.com/tutorial-demos/html5-video" class="button button--inline-block button--medium">View Demo</a>
</p>
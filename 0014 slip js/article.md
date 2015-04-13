[Here’s the link to the github repository](https://github.com/pornel/slip), where you can download the library and read the documentation. This is highly useful, especially as users are becoming more and more acquainted with native gestures on their devices.

## Example Of Native Swiping

A perfect example of a native swipe method that iPhone users would be accustomed to exists in the messages app. In the inbox view, if you swipe a message to the left, you get the option to delete it.

## Ideal Scenario

I’ve developed apps on PhoneGap before. PhoneGap is a framework that helps you streamline app development  and deployment for mobile devices by using HTML, CSS, and JavaScript. It acts as a “gap” between your HTML code and the device’s native code base and API’s. The tricky part about developing with PhoneGap is making your app not feel like a local website. For the most part, this is done by using CSS3 transitions and such, but the gestures that device users are accustomed to are what really makes an app stand apart. Getting a native feel in an HTML app can be a challenge, but Slip.js gives us some serious help here with the native-feeling swiping and re-ordering.

## What I Like

After testing out the demo on my mobile device by loading [this url](http://pornel.net/slip/) into my browser, I noticed immediately how smoothly and responsively Slip.js reacts. The swiping felt native, and there was no jittering. I liked the touch-hold to reorder as well, which I thought could come in handy for any developer who needs to implement some ordered-type functionality in their app. If I develop any more PhoneGap apps in the future, I’ll definitely be keeping this on my radar.

## Some Minor Issues

Even though it’s nice that you can still select text on a list element, swiping it out with selected text is kind of weird. You can work around this by disabling selections with CSS, so that’s nothing to worry about. If you’re developing a PhoneGap app, you’ll want all text not selectable anyway. The only other bug (that is already documented by the developer) is getting the page to scroll when re-ordering further down the page. When this bug gets worked out, this will be a very complete solution and a great addition to any mobile app/web page.

<p class="text-align--center">
<a href="https://github.com/pornel/slip" class="button button--inline-block button--medium">Get SlipJS Here</a>
</p>
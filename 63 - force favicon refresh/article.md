Those pesky little favicons are so microscopic in the grander scheme of things, yet they somehow manage to annoy us. Who would've thought that to really cover all bases, we would need a million different icon files and a ridiculous addition to the `head` tags in our document? Not to mention, good luck trying to refresh your favicon. 

Speaking of refreshing favicons, I came across this handy snippet the other day that basically forces favicons to refresh once and for all. It's ridiculously simple, and will save you a few grey hairs. Here it is:

```html
<!-- append ?v=2 to icon url -->
<link rel="shortcut icon" href="/favicon.ico?v=2">
```

That's it. Easy. It pretty much forces the browser to download a new favicon due to the query string. Source - [Stack Overflow](http://stackoverflow.com/questions/2208933/how-do-i-force-a-favicon-refresh).
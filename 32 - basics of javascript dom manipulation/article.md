All the JavaScript DOM manipulation methods presented here are well documented on the Mozilla Developer Network site. This is just a handy quick reference to see them all on one page, with links to the official reference pages for further documentation.

## 1) Creation of a DOM Element ([MDN Reference](https://developer.mozilla.org/en-US/docs/Web/API/document.createElement))

In an HTML document creates the specified HTML element or `HTMLUnknownElement` if the element is not known.

```language-javascript
var element = document.createElement(tagName);
```

* `element` is the created element object.
* `tagName` is a string that specifies the type of element to be created. The nodeName of the created element is initialized with the value of `tagName`. Don’t use qualified names (like “`html:a`”) with this method.

## 2) Addition of Created Element to DOM ([MDN Reference](https://developer.mozilla.org/en-US/docs/Web/API/Node.appendChild))

Adds a node to the end of the list of children of a specified parent node. If the node already exists it is removed from current parent node, then added to new parent node.

```language-javascript
var child = element.appendChild(child);
```

* `element` is the parent element.
* `child` is the node to append underneath `element`. Also returned.

## 3) Styling an HTML Element ([MDN Reference](https://developer.mozilla.org/en-US/docs/Web/API/HTMLElement.style))

The `HTMLElement.style` property is an object that represents the element’s style attribute. To get the values of all CSS properties for an element you should use `window.getComputedStyle()` instead.

```language-javascript
element.style.color = "#ff3300";
element.style.marginTop = "30px";
element.style.paddingBottom = "30px";
```

## 4) Getting and Setting the HTML Elements ([MDN Reference](https://developer.mozilla.org/en-US/docs/Web/API/Element.innerHTML))

`innerHTML` sets or gets the HTML syntax describing the element’s descendants.

```language-javascript
// get the HTML of "element"
var content = element.innerHTML;

// set the HTML of "otherElement"
otherElement.innterHTML = content;
```

## 5) Getting and Setting the Class Name ([MDN Reference](https://developer.mozilla.org/en-US/docs/Web/API/Element.className))

`className` gets and sets the value of the class attribute of the specified element.

```language-javascript
// get the class name of "element"
var cName = element.className;

// set the class name of "otherElement"
otherElement.className = cName;
```

## 6) Getting and Setting the ID ([MDN Reference](https://developer.mozilla.org/en-US/docs/Web/API/Element.id))

Gets or sets the element’s identifier (attribute id).

```language-javascript
// get the id of "element"
var idStr = element.id;

// set the id of "otherElement"
otherElement.id = idStr;
```

## 7) Accessing DOM Elements

Various ways to access DOM elements.

### 7a) Accessing by ID ([MDN Reference](https://developer.mozilla.org/en-US/docs/Web/API/document.getElementById))

Returns a reference to the element by its ID.

```language-javascript
element = document.getElementById(id);
```

### 7b) Accessing by Class Names ([MDN Reference](https://developer.mozilla.org/en-US/docs/Web/API/document.getElementsByClassName))

Returns an array of all child elements which have all of the given class names. This is not supported from IE8 and below, so be careful when using it.

```language-javascript
elements = document.getElementsByClassName(names); // or:
elements = rootElement.getElementsByClassName(names);
```

* `elements` is a `HTMLCollection` of found elements.
* `names` is a string representing the list of class names to match; class names are separated by whitespace
* `getElementsByClassName` can be called on any element, not only on the document. The element on which it is called will be used as the root of the search.

### 7c) Accessing by Tag Name ([MDN Reference](https://developer.mozilla.org/en-US/docs/Web/API/document.getElementsByTagName))

Returns an HTMLCollection of elements with the given tag name. The complete document is searched, including the root node.

```language-javascript
var elements = document.getElementsByTagName(name);
```

* elements is a live HTMLCollection (but see the note below) of found elements in the order they appear in the tree.
* name is a string representing the name of the elements. The special string "\*" represents all elements.

### 7d) Accessing First Found Selector ([MDN Reference](https://developer.mozilla.org/en-US/docs/Web/API/document.querySelector))

Returns the first element within the document (using depth-first pre-order traversal of the document’s nodes) that matches the specified group of selectors. Supported by IE8 and up.

```language-javascript
element = document.querySelector(selectors);
```

* `element` is an element object.
* `selectors` is a string containing one or more CSS selectors separated by commas.

In this example, the first element in the document with the class “myclass” is returned:

```language-javascript
var el = document.querySelector(".myclass");
```

### 7e) Accessing an Array of Selectors ([MDN Reference](https://developer.mozilla.org/en-US/docs/Web/API/document.querySelectorAll))

Returns a list of the elements within the document (using depth-first pre-order traversal of the document’s nodes) that match the specified group of selectors. The object returned is a NodeList. Supported by IE8 and up.

```language-javascript
elementList = document.querySelectorAll(selectors);
```

* `elementList` is a non-live NodeList of element objects.
* `selectors` is a string containing one or more CSS selectors separated by commas.

This example returns a list of all div elements within the document with a class of either “note” or “alert“:

```language-javascript
var matches = document.querySelectorAll("div.note, div.alert");
```

## 8) Relationships

DOM element relationships, and accessing elements in relation to other elements.

### 8a) Children of a Given Element ([MDN Reference](https://developer.mozilla.org/en-US/docs/Web/API/Node.childNodes))

`childNodes` returns a collection of child nodes of the given element.

```language-javascript
var ndList = elementNodeReference.childNodes;
```

### 8b) Next Sibling of a Given Element ([MDN Reference](https://developer.mozilla.org/en-US/docs/Web/API/Node.nextSibling))

Returns the node immediately following the specified one in its parent’s childNodes list, or null if the specified node is the last node in that list.

```language-javascript
nextNode = node.nextSibling
```

### 8c) Child Elements of a Given Object ([MDN Reference](https://developer.mozilla.org/en-US/docs/Web/API/ParentNode.children))

The `ParentNode.children` read-only property returns a live `HTMLCollection` of child elements of the given object. The items in the returned collection are objects and not strings. To get data from those node objects, you must use their properties (e.g. `elementNodeReference.children[1].nodeName` to get the name, etc.).

```language-javascript
var elList = elementNodeReference.children;
```

### 8d) Element Immediately Following Specified Element ([MDN Reference](https://developer.mozilla.org/en-US/docs/Web/API/ChildNode.nextElementSibling))

The `ChildNode.nextElementSibling` read-only property returns the element immediately following the specified one in its parent’s children list, or null if the specified element is the last one in the list.

```language-javascript
var nextNode = elementNodeReference.nextElementSibling;
```
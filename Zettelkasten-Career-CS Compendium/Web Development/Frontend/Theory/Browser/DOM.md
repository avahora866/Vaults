---
tags:
  - WebDevelopment
---

-------------------------------------------------------------------------------
# Introduction to the DOM

The **Document Object Model** (_DOM_) is the data representation of the objects that comprise the structure and content of a document on the web. This guide will introduce the DOM, look at how the DOM represents an [HTML](https://developer.mozilla.org/en-US/docs/Glossary/HTML) document in memory and how to use APIs to create web content and applications.

# What is the DOM

The Document Object Model (DOM) is a programming interface for web documents. It represents the page so that programs can change the document structure, style, and content. The DOM represents the document as nodes and objects; that way, programming languages can interact with the page.

A web page is a document that can be either displayed in the browser window or as the HTML source. In both cases, it is the same document but the Document Object Model (DOM) representation allows it to be manipulated. As an object-oriented representation of the web page, it can be modified with a scripting language such as JavaScript.

For example, the DOM specifies that the `querySelectorAll` method in this code snippet must return a list of all the `[<p>](<https://developer.mozilla.org/en-US/docs/Web/HTML/Element/p>)` elements in the document:

```
const paragraphs = document.querySelectorAll("p");
// paragraphs[0] is the first <p> element
// paragraphs[1] is the second <p> element, etc.
alert(paragraphs[0].nodeName);
```
All of the properties, methods, and events available for manipulating and creating web pages are organized into objects. For example, the `document` object that represents the document itself, any `table` objects that implement the `[HTMLTableElement](<https://developer.mozilla.org/en-US/docs/Web/API/HTMLTableElement>)` DOM interface for accessing HTML tables, and so forth, are all objects.

The DOM is built using multiple APIs that work together. The core [DOM](https://developer.mozilla.org/en-US/docs/Web/API/Document_Object_Model) defines the entities describing any document and the objects within it. This is expanded upon as needed by other APIs that add new features and capabilities to the DOM. For example, the [HTML DOM API](https://developer.mozilla.org/en-US/docs/Web/API/HTML_DOM_API) adds support for representing HTML documents to the core DOM, and the SVG API adds support for representing SVG documents.
# DOM and JavaScript

The previous short example, like nearly all examples, is [JavaScript](https://developer.mozilla.org/en-US/docs/Glossary/JavaScript). That is to say, it is _written_ in JavaScript, but _uses_ the DOM to access the document and its elements. The DOM is not a programming language, but without it, the JavaScript language wouldn't have any model or notion of web pages, HTML documents, SVG documents, and their component parts. The document as a whole, the head, tables within the document, table headers, text within the table cells, and all other elements in a document are parts of the document object model for that document. They can all be accessed and manipulated using the DOM and a scripting language like JavaScript.

The DOM is not part of the JavaScript language, but is instead a Web API used to build websites. JavaScript can also be used in other contexts. For example, Node.js runs JavaScript programs on a computer, but provides a different set of APIs, and the DOM API is not a core part of the Node.js runtime.

The DOM was designed to be independent of any particular programming language, making the structural representation of the document available from a single, consistent API. Even if most web developers will only use the DOM through JavaScript, implementations of the DOM can be built for any language, as this Python example demonstrates:
```Python
 Python DOM example
import xml.dom.minidom as m
doc = m.parse(r"C:\Projects\Py\chap1.xml")
oc.nodeName # DOM property of document object
p_list = doc.getElementsByTagName("para")
```
# Accessing the DOM

You don't have to do anything special to begin using the DOM. You use the API directly in JavaScript from within what is called a _script_, a program run by a browser.

When you create a script, whether inline in a `<script>` element or included in the web page, you can immediately begin using the API for the `[document](<https://developer.mozilla.org/en-US/docs/Web/API/Document>)` or `[window](<https://developer.mozilla.org/en-US/docs/Web/API/Window>)` objects to manipulate the document itself, or any of the various elements in the web page (the descendant elements of the document). Your DOM programming may be something as simple as the following example, which displays a message on the console by using the `[console.log()](<https://developer.mozilla.org/en-US/docs/Web/API/console/log>)` function:
```
<body onload="console.log('Welcome to my home page!');">
  …
</body>
```
As it is generally not recommended to mix the structure of the page (written in HTML) and manipulation of the DOM (written in JavaScript), the JavaScript parts will be grouped together here, and separated from the HTML.
For example, the following function creates a new [h1](https://developer.mozilla.org/en-US/docs/Web/HTML/Element/Heading_Elements) element, adds text to that element, and then adds it to the tree for the document:
```html
<html lang="en">
  <head>
    <script>
      // run this function when the document is loaded
      window.onload = () => {
        // create a couple of elements in an otherwise empty HTML page
        const heading = document.createElement("h1");
        const headingText = document.createTextNode("Big Head!");
        heading.appendChild(headingText);
        document.body.appendChild(heading);
      };
    </script>
  </head>
  <body></body>
</html>
```
# Fundamental Data types

This page tries to describe the various objects and types in simple terms. But there are a number of different data types being passed around the API that you should be aware of.

> [!NOTE] Note:
> Because the vast majority of code that uses the DOM revolves around manipulating HTML documents, it's common to refer to the nodes in the DOM as elements, although strictly speaking not every node is an element.

The following table briefly describes these data types.

|Data type (Interface)|Description|
|---|---|
|[`Document`](https://developer.mozilla.org/en-US/docs/Web/API/Document)|When a member returns an object of type `document` (e.g., the `ownerDocument` property of an element returns the `document` to which it belongs), this object is the root `document` object itself. The [DOM `document` Reference](https://developer.mozilla.org/en-US/docs/Web/API/Document) chapter describes the `document` object.|
|[`Node`](https://developer.mozilla.org/en-US/docs/Web/API/Node)|Every object located within a document is a node of some kind. In an HTML document, an object can be an element node but also a text node or attribute node.|
|[`Element`](https://developer.mozilla.org/en-US/docs/Web/API/Element)|The `element` type is based on `node`. It refers to an element or a node of type `element` returned by a member of the DOM API. Rather than saying, for example, that the [`document.createElement()`](https://developer.mozilla.org/en-US/docs/Web/API/Document/createElement) method returns an object reference to a `node`, we just say that this method returns the `element` that has just been created in the DOM. `element` objects implement the DOM `Element` interface and also the more basic `Node` interface, both of which are included together in this reference. In an HTML document, elements are further enhanced by the HTML DOM API's [`HTMLElement`](https://developer.mozilla.org/en-US/docs/Web/API/HTMLElement) interface as well as other interfaces describing capabilities of specific kinds of elements (for instance, [`HTMLTableElement`](https://developer.mozilla.org/en-US/docs/Web/API/HTMLTableElement) for [`<table>`](https://developer.mozilla.org/en-US/docs/Web/HTML/Element/table) elements).|
|[`NodeList`](https://developer.mozilla.org/en-US/docs/Web/API/NodeList)|A `nodeList` is an array of elements, like the kind that is returned by the method [`document.querySelectorAll()`](https://developer.mozilla.org/en-US/docs/Web/API/Document/querySelectorAll). Items in a `nodeList` are accessed by index in either of two ways:<br><br>- list.item(1)<br>- list[1]<br><br>These two are equivalent. In the first, `item()` is the single method on the `nodeList` object. The latter uses the typical array syntax to fetch the second item in the list.|
|[`Attr`](https://developer.mozilla.org/en-US/docs/Web/API/Attr)|When an `attribute` is returned by a member (e.g., by the `createAttribute()` method), it is an object reference that exposes a special (albeit small) interface for attributes. Attributes are nodes in the DOM just like elements are, though you may rarely use them as such.|
|[`NamedNodeMap`](https://developer.mozilla.org/en-US/docs/Web/API/NamedNodeMap)|A `namedNodeMap` is like an array, but the items are accessed by name or index, though this latter case is merely a convenience for enumeration, as they are in no particular order in the list. A `namedNodeMap` has an `item()` method for this purpose, and you can also add and remove items from a `namedNodeMap`.|

There are also some common terminology considerations to keep in mind. It's common to refer to any [Attr](https://developer.mozilla.org/en-US/docs/Web/API/Attr) node as an attribute, for example, and to refer to an array of DOM nodes as a nodeList. You'll find these terms and others to be introduced and used throughout the documentation.
[[DOM|https://developer.mozilla.org/en-US/docs/Web/API/Document_Object_Model/Introduction]]

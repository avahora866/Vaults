---
tags:
  - JavaScript
  - WebDevelopment
---
A bookmarklet is a small piece of JavaScript code that is saved as a bookmark in a web browser. Unlike a typical bookmark that simply links to a webpage, a bookmarklet is a mini-program that can perform an action on the current webpage when clicked. 

Here's a simple example of a bookmarklet that changes the background color of a webpage:

```javascript
javascript:(function(){document.body.style.backgroundColor='yellow';})();
```

When you save this as a bookmark and click it on any webpage, the page's background will turn yellow. 

The key characteristics of a bookmarklet are:
- It starts with `javascript:` 
- It contains a self-executing JavaScript function
- It can manipulate the current webpage
- It runs entirely in the browser without loading a new page

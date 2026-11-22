---
tags:
  - FORMAT
---
In [JavaScript](https://developer.mozilla.org/en-US/docs/Glossary/JavaScript), a truthy value is a value that is considered `true` when encountered in a [Boolean](https://developer.mozilla.org/en-US/docs/Glossary/Boolean) context. All values are truthy unless they are defined as [falsy](https://developer.mozilla.org/en-US/docs/Glossary/Falsy). That is, all values are _truthy_ except `false, 0, -0, 0n, "", null, undefined, and NaN`.

[JavaScript](https://developer.mozilla.org/en-US/docs/Glossary/JavaScript) uses [type coercion](https://developer.mozilla.org/en-US/docs/Glossary/Type_coercion) in Boolean contexts.

Examples of _truthy_ values in JavaScript (which will be coerced to `true` in Boolean contexts, and thus execute the `if` block):

```
if (true)
if ({}) 
if ([]) 
if (42) 
if ("0") 
if ("false") 
if (new Date()) 
if (-42) 
if (12n) 
if (3.14) 
if (-3.14) 
if (Infinity) 
if (-Infinity)
```
(https://developer.mozilla.org/en-US/docs/Glossary/Truthy)
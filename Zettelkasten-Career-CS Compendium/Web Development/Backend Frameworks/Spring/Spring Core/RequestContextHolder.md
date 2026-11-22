---
tags:
  - Spring
  - LEAF
---
Spring utility class that provides access to request related information.  
It has a method called \`getRequestAttributes()\` which returns \`RequestAttributes\` which holds request scoped data.

# Key Points

* Only works inside a web request context (i.e., within an HTTP request).  
* Returns null if called outside of a request.  
* Useful for retrieving request-scoped data in non-Spring-managed components.

# Spring managed components

In spring managed components you should use the \`@RequestScope\` annotation and autowire in the bean to where you want to use it  

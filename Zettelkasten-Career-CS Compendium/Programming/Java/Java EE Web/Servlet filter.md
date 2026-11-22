---
tags:
  - Java
---
A filter is an object that is invoked at the pre-processing and postprocessing of a request on the server, i.e., before and after the execution of a servlet for filtering the request. Filter API (or interface) includes some methods which help us in filtering requests.

Advantages of using filter

Authentication and authorization of requests for resources. (To check whether the user is valid or not and then forward its request.)  
Formatting of request body or header before sending it to the servlet. (To format the unformatted data)  
Compressing the response data sent to the client. (e.g., encrypting)  
Alter the response by adding some cookies, header information, etc.  
Input validation. (Most imp)

Filter API concepts  
Three methods – init(), doFilter(), destroy (). Have to override these methods. They are the lifecycle methods of a Filter.  
doFilter will be executed in both preprocessing \+ postprocessing.  
doFilter() method takes three arguments – ServletRequest, ServletResponse, FilterChain. With the help of FilterChain, we can forward the request after successful authentication.  

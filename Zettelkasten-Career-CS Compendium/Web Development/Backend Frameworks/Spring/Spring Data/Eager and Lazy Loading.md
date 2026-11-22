---
tags:
  - Spring
---
When we fetch an entity it will by default not fetch any foriegn entities is is related. This is called **Lazy Loading**. 
The inverse where when you fetch an entity and it fetches all the entites it is related to is called **Eager Loading**.
**Note: The eager and lazy loading only comes into play if the relationship is uni-directional and the entity owns the relationship or if the relationship is bi-directional**
We can set the type of loading through an attribute on the annotation
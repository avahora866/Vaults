---
tags:
  - Spring
---
# Object Relational Mapping (ORM)
ORM is a tool used to map java classess to database. It helps in removing the need for developers to create low-level SQL queries (CREATE, DELETE, UPDATE, etc)
# Jakarta Persitance API (JPA)
Is a industry standerd way to develop an ORM tool. It doesn't actually map the classes to the database it gives a list of interfaces for a vendor to implement and they develop the ORM. **JPA IS A SPECIFICATION NOT AN IMPLEMENTATION**
By having a standard API you are not locked into the vendors implantation (If hibernate shuts down we can simply move to another JPA vendor easily) (No vendor Lock in), possible as we are coding the JPA specification which Hibernate implements

# Hibernate
Is the most popular JPA Vendor (ORM). It follows the JPA standerd. Spring uses Hibernate by default as its ORM tool
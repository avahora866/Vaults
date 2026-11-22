---
tags:
  - Spring
---
Propagation represents the business logic transaction boundary. It lets Spring know when to start and close a transaction. Some databases have features that mimic this feature (savepoints) but its mainly a web framework or java based concept.  
In spring the propagation level can be set using an attribute caled propagation on the @Transactional. 

# Required

Default setting for Spring’s propagation.  
checks if there is an active transaction, and if nothing exists, it creates a new one. Otherwise, the business logic appends to the currently active transaction

# Supports

For SUPPORTS, Spring first checks if an active transaction exists. If a transaction exists, then the existing transaction will be used. If there isn’t a transaction, it is executed non-transactional:

# Mandatory

When the propagation is MANDATORY, if there is an active transaction, then it will be used. If there isn’t an active transaction, then Spring throws an exception:

# Never

For transactional logic with NEVER propagation, Spring throws an exception if there’s an active transaction:

# Not Supported

If a current transaction exists, first Spring suspends it, and then the business logic is executed without a transaction:

# Requires New

When the propagation is REQUIRES\_NEW, Spring suspends the current transaction if it exists, and then creates a new one:

# Nested

For NESTED propagation, Spring checks if a transaction exists, and if so, it marks a save point. This means that if our business logic execution throws an exception, then the transaction rollbacks to this save point. If there’s no active transaction, it works like REQUIRED.  

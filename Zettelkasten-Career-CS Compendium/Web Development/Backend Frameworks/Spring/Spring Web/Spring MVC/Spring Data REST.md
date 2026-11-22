---
tags:
  - Spring
---
Its a way to generate endpoints leveraging your current JPA repositories performing the basic CRUD features.
It will scan your project for `JpaRepositories` and expose [[Rest|REST]] Endpoints for each entity specified.
It will create the endpoints based on the entity name with an s at the end, so employee will become `/employees`. It does not handle complex pluralisation i.e. `Goose -> Geese`.
To use Spring Data Rest simply add the dependency in your pom file
You can also specify the default base path in our application properties which would make your endpoint look like this: `http://localhost:8080/custom-base-path/employees`
# HATEOS (Hypermedia as the Engine of Application State)
Spring Data REST endpoints are HATEOS compliant, meaning they provide information to access REST interfaces (Metadata for the REST data)

# Customizing endpoints
`@RepositoryRestResource(path="\geese")`
# Pagination
By default Spring Data REST will return the first 20 elements.
You an navigate to different page using query params i.e.: `http://localhost:8080/employees?page=0`
You can configure this in the application properties (default size of pages, max size of pages)
# Sorting
Sorting is performed according to the entities field names through query params: `http://localhost:8080/employees?sort=lastName,desc` (ASC is default) / `http://localhost:8080/employees?sort=lastName,firstName,asc`
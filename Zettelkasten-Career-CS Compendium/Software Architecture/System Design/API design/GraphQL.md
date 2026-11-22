---
tags:
  - APIDesign
---
GraphQL is a query language for APIs and a runtime for executing those queries against your data. Developed by Facebook in 2012 and released publicly in 2015, GraphQL provides an alternative to REST for API development.

# Core Concepts of GraphQL

1. **Single Endpoint**: Unlike REST with multiple endpoints, GraphQL APIs typically have a single endpoint that accepts query requests.
2. **Client-Specified Queries**: Clients precisely specify what data they need, eliminating over-fetching and under-fetching issues common in REST.
3. **Strongly-Typed Schema**: Every GraphQL API defines a schema that describes all possible data that can be queried, providing built-in documentation and type safety.
4. **Hierarchical Structure**: Queries mirror the shape of the resulting JSON data, making responses predictable and intuitive.
5. **Real-time Updates**: Support for subscriptions allows servers to push updates to clients when data changes.

# GraphQL in Practice

A typical GraphQL operation looks like this:

```graphql
query {
  user(id: "123") {
    name
    email
    posts {
      title
      commentCount
    }
    followers(first: 3) {
      name
    }
  }
}
```

This single request retrieves a user's information, their posts with titles and comment counts, and their first three followers—data that might require multiple REST API calls.

# Advantages over REST

- **No Over-fetching**: Clients retrieve exactly what they need, nothing more.
- **No Under-fetching**: Eliminates the need for multiple requests to gather related data.
- **Evolution Without Versioning**: APIs can evolve without breaking existing queries.
- **Introspection**: The schema system provides automatic documentation.
- **Strong Developer Tools**: The type system enables powerful tooling and IDE integrations.

GraphQL has gained significant adoption among companies like GitHub, Shopify, Twitter, and Airbnb, especially for complex applications where efficient data loading is critical.
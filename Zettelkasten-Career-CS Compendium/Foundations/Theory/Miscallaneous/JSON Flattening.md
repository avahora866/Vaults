---
tags:
  - Theory
---
Flattening a JSON object involves converting a nested structure into a flat one. This is typically done for several reasons:

1. **Data Storage and Processing**  
   Flattening makes it easier to store JSON data in tabular formats like relational databases or spreadsheets, which generally don’t support nested structures.  
2. **Data Analysis**  
   Many data analysis tools expect flat structures (rows and columns). Flattening allows nested data to be easily analyzed and queried.  
3. **Integration with APIs**  
   Some APIs or libraries expect flat key-value pairs instead of complex nested objects.  
4. **Ease of Indexing**  
   Flattening helps in indexing fields for fast lookups, especially in systems like Elasticsearch.

# Before Flattening

```json
{
  "user": {
    "id": 123,
    "name": {
      "first": "John",
      "last": "Doe"
    },
    "email": "john.doe@example.com"
  },
  "status": "active"
}
```

# Flattened

```json
{
  "user.id": 123,
  "user.name.first": "John",
  "user.name.last": "Doe",
  "user.email": "john.doe@example.com",
  "status": "active"
}
```


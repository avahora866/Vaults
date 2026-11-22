---
tags:
  - FORMAT
  - SystemDesign
---
**Pagination** is the process of splitting a large set of results (e.g., database records or API responses) into smaller chunks ("pages") to improve performance and usability.

---

# Why it’s used:

- Prevents sending too much data at once
- Reduces memory, bandwidth, and load on both client and server
- Allows users to browse results incrementally

---

# Common Pagination Methods:

1. **Offset-based**
    
    ```
    GET /items?limit=10&offset=20
    ```
    
    - Returns 10 items, starting from item #21
    - Pros: Simple and widely supported
    - Cons: Poor performance on large datasets (especially for deep pages)
2. **Page-based**
    
    ```
    GET /items?page=3&per_page=10
    ```
    
    - Like offset, but uses page numbers instead of raw offsets
    - Pros: User-friendly for UIs
    - Cons: Same performance issues as offset
3. **Cursor-based (a.k.a. keyset pagination)**
    
    ```
    GET /items?after=abc123
    ```
    
    - Uses a unique ID or timestamp from the last item to get the next page
    - Pros: Faster for large or real-time data; consistent with updates
    - Cons: More complex to implement and maintain
4. **Token-based (opaque cursor)**
    
    ```
    GET /items?page_token=eyJpZCI6MTAwfQ==
    ```
    
    - Server returns an opaque token to fetch the next page
    - Pros: Hides internal structure; safer and flexible
    - Cons: Debugging is harder

---

# Good API Practice:

- Always return:
    - `items`: the data
    - `total` or `has_more`: whether there’s more data
    - `next` link or `cursor`: how to get the next page

Example:

```json
{
  "items": [...],
  "next": "/items?after=xyz789",
  "has_more": true
}
```

Want a sample implementation in a specific language or framework?
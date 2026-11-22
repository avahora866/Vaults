---
tags:
  - DataStructure
---
# `compute`

```java
map.compute(key, (k, v) -> newValue);
```

- It **always runs the lambda**, whether the key is present or not.
    
- If the lambda returns `null`, the key is removed from the map.
    

## Use case:

When you want to **always compute a new value**, even if the key doesn’t exist.

```java
map.compute("apple", (k, v) -> (v == null) ? 1 : v + 1);
```

This is basically the same as using `getOrDefault()` but more atomic/thread-safe if needed.

---

# `computeIfPresent`

```java
map.computeIfPresent(key, (k, v) -> newValue);
```

- Only computes a new value **if the key is already present**.
    
- If the lambda returns `null`, the key is removed from the map.
    

## Use case:

When you want to **update an existing value**, but **do nothing** if the key isn’t in the map.

```java
map.computeIfPresent("apple", (k, v) -> v + 1);
```

If `"apple"` isn’t in the map, this does nothing.

---

# `computeIfAbsent`

```java
map.computeIfAbsent(key, k -> value);
```

- Only computes and inserts a value **if the key is not already present**.
    

## Use case:

When you want to **insert a default value** if the key doesn’t exist.

```java
map.computeIfAbsent("apple", k -> 0);
```

This will only set `"apple"` to 0 if it’s not already in the map.

---

# Summary

|Method|Runs If Key Exists|Runs If Key Missing|Can Remove Key|Typical Use Case|
|---|---|---|---|---|
|`compute`|✅|✅|✅|Update or insert with full control|
|`computeIfPresent`|✅|❌|✅|Update existing value|
|`computeIfAbsent`|❌|✅|❌|Insert default value if missing|

---
tags:
  - Spring
---
Caching logic in Spring is applied transparently without any interference from the invoker (code that calls the cached method)

1. Add the @EnableCaching annotation  
2. Add the @Cacheble annotation to the method you want to cache

```java
@Cacheble("caching-example")
public void method(int intParam)
```

Before executing the method, the Spring caching abstraction will look for a caching-example entry with an intParam argument.   
If an entry is found the content of the entry is returned to the invoker and the method is **NOT** executed. If an entry is found then the method is executed **AND** the cache is updated before returning the method to the invoker.

Spring has a default cache library (provides the actual caching functionality) called simple provider. It is not recommended to use the simple provider cache library for production as it lacks persistence, eviction policies, and distributed capabilities.

When a cache is required the provider creates it for you (creates the cache entry)  
Spring cache abstraction relies on the implementation of the following interface:

* Cache  
* CacheManager

If spring cant find any bean that implements the above interfaces or the CacheResolver interface it will try the following providers who implement the interfaces in order.

Supported Cache Providers:

1. Generic  
2. JCache (JSR-107) (EhCache 3, Hazelcast, Infinispan, and others)  
3. Hazelcast  
4. Infinispan  
5. Couchbase  
6. Redis  
7. Caffeine  
8. Cache2k  
9. Simple

# SimpleProvider

Springs default CacheLibray that is not recommended for production for the following reasons:

## Persistence

Persistence refers to the ability of the cache to **retain data** even after the application is stopped or restarted. 

* **Simple in-memory cache** (default Spring Boot provider) stores data **only in memory** (e.g., using a `ConcurrentHashMap`), meaning that once the application is restarted, the cache is emptied, and all cached data is lost.  
* **Production-level cache providers** (e.g., Redis, EhCache, Caffeine) often provide **persistence options**, where data can be written to disk or external storage (e.g., Redis can persist to disk, EhCache can use disk-based storage). This ensures that cache data survives application restarts or server failures.

## Eviction

Eviction refers to the strategy used to **remove data** from the cache when it is full or when certain conditions are met. Caching is most effective when it stores a limited amount of data and removes items that are no longer useful or are stale.

* **Simple in-memory cache** does not have advanced eviction policies, so it will just grow in memory until the application runs out of memory (if the cache is not manually cleared).  
* **Eviction policies** include strategies like:  
  * **Least Recently Used (LRU)**: Removes the least recently accessed items.  
  * **Time-to-Live (TTL)**: Items are removed after a certain time period.  
  * **Maximum Size**: Once the cache exceeds a certain size, the least useful data is removed.  
* **Production-level cache providers** typically implement sophisticated eviction policies to manage memory usage and ensure that only the most relevant data stays in the cache.

## Distributed Capabilities:

Distributed capabilities refer to the ability of a cache to be shared across **multiple application instances** or across different machines. This is particularly important in a **cloud-based** or [[Microservices Architecture|microservices architecture]] where multiple instances of the application might need to access the same cache.

* **Simple in-memory cache** is local to the **JVM** where it’s running. It means that each instance of your application will have its own separate in-memory cache, and data won’t be shared across instances. This could lead to **inconsistent cache state** in a multi-instance environment.  
* **Distributed cache providers** (e.g., **Redis**, **Hazelcast**, **Couchbase**) are designed to allow **multiple application instances** to share the same cache, ensuring that cached data is available across all instances in a **consistent manner**. Distributed caches are crucial when you have applications running on multiple servers or across multiple machines
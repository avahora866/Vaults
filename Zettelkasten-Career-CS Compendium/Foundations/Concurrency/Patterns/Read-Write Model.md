---
tags:
  - FLASHCARD
  - Concurrency
---
The Read-Write Lock pattern is a synchronization primitive that improves performance in read-heavy scenarios. It maintains a pair of associated locks—one for reading and one for writing—which allows for a higher level of concurrency than a standard mutex.

- Read Lock (Shared): Multiple threads can acquire the read lock at the same time, as long as no thread holds the write lock.
- Write Lock (Exclusive): Only one thread can acquire the write lock at a time, and it can only do so if no other thread holds either a read or a write lock.

# Explanation
This pattern is ideal for data structures that are read far more often than they are modified (e.g., an in-memory cache).

A standard lock (synchronized or ReentrantLock) is exclusive. If 100 threads need to read a value, they must line up and access it one by one. This is inefficient, as read operations do not interfere with each other.

A ReadWriteLock solves this.
- If 100 threads want to read: All 100 can acquire the read lock and access the data concurrently.
- If 1 thread wants to write: It must wait for all 100 readers to finish. Once it acquires the write lock, any new readers (or other writers) must wait for it to complete its write.

This provides the safety of an exclusive lock for writing while allowing maximum performance for reading.

# Code
```java
import java.util.HashMap;
import java.util.Map;
import java.util.concurrent.ExecutorService;
import java.util.concurrent.Executors;
import java.util.concurrent.TimeUnit;
import java.util.concurrent.locks.Lock;
import java.util.concurrent.locks.ReadWriteLock;
import java.util.concurrent.locks.ReentrantReadWriteLock;

/**
 * A thread-safe cache implemented with a ReadWriteLock.
 * Perfect for data that is read often but written infrequently.
 */
class ReadWriteCache<K, V> {

    private final Map<K, V> map = new HashMap<>();
    
    // 1. Create the ReadWriteLock instance
    private final ReadWriteLock rwLock = new ReentrantReadWriteLock();
    
    // 2. Extract the individual read and write locks
    private final Lock readLock = rwLock.readLock();
    private final Lock writeLock = rwLock.writeLock();

    /**
     * Adds a value to the cache. This is a write operation.
     */
    public void put(K key, V value) {
        // 3. Acquire the exclusive write lock
        writeLock.lock();
        try {
            System.out.println(Thread.currentThread().getName() + " [WRITING]");
            Thread.sleep(100); // Simulate slow write
            map.put(key, value);
        } catch (InterruptedException e) {
            Thread.currentThread().interrupt();
        } finally {
            // 4. Always release the lock in a finally block
            writeLock.unlock();
        }
    }

    /**
     * Gets a value from the cache. This is a read operation.
     */
    public V get(K key) {
        // 5. Acquire the shared read lock
        readLock.lock();
        try {
            System.out.println(Thread.currentThread().getName() + " (Reading)");
            Thread.sleep(50); // Simulate read time
            return map.get(key);
        } catch (InterruptedException e) {
            Thread.currentThread().interrupt();
            return null;
        } finally {
            // 6. Always release the lock in a finally block
            readLock.unlock();
        }
    }
}

public class ReadWriteLockDemo {
    public static void main(String[] args) {
        ReadWriteCache<String, String> cache = new ReadWriteCache<>();
        ExecutorService executor = Executors.newFixedThreadPool(10);

        // Submit one writer task
        executor.submit(() -> cache.put("key1", "value1"));

        // Submit multiple reader tasks
        for (int i = 0; i < 5; i++) {
            executor.submit(() -> cache.get("key1"));
        }
        
        // Submit another writer task
        executor.submit(() -> cache.put("key2", "value2"));
        
        // Submit more reader tasks
        for (int i = 0; i < 5; i++) {
            executor.submit(() -> cache.get("key2"));
        }

        try {
            executor.shutdown();
            executor.awaitTermination(1, TimeUnit.MINUTES);
        } catch (InterruptedException e) {
            e.printStackTrace();
        }
    }
}
```

## Code Explanation
1. We initialize a ReentrantReadWriteLock, which is the standard Java implementation of the ReadWriteLock interface.
2. We get the two specialized locks from it: readLock and writeLock.
3. In the put (write) method, we use writeLock.lock(). This is an exclusive lock. No other thread (reader or writer) can enter any critical section while this lock is held.
4. The finally block ensures the write lock is always released, even if an exception occurs.
5. In the get (read) method, we use readLock.lock(). This is a shared lock. Other threads can also acquire the readLock at the same time. However, any thread trying to get the writeLock will be blocked until all read locks are released.
6. The finally block ensures the read lock is released.

When you run this code, you will see that all the "Reading" threads can run concurrently in batches, but they will be forced to wait while a "[WRITING]" thread is active.

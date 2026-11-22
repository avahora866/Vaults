---
tags:
  - SystemDesign
---

A performance bottleneck occurs when a specific part of a system limits overall throughput or responsiveness. The slowest component becomes the constraint. The three primary types are CPU, memory, and I/O bottlenecks.

---
# CPU Bottlenecks

## Description
The CPU is fully utilized while other resources (memory, disk, network) remain underused. This slows down task processing and leads to reduced performance.

## Symptoms
- CPU usage consistently near or at 100%
- Slower application response times, especially under load
- Increased latency during computation-heavy operations
- Fans running loudly or system heating up

## Common Causes
- Inefficient or unoptimized code (e.g. loops, recursion)
- Excessive use of single-threaded operations
- Lack of parallelism (not using multiple cores)
- Poor CPU scheduling or high-priority background tasks
- Running too many processes or containers

## Detection Tools
- Linux: `top`, `htop`, `mpstat`, `perf`
- Windows: Task Manager, Resource Monitor
- Profilers: VisualVM, Intel VTune, gprof, perf

## Fixes
- Optimize algorithms and data structures
- Use multi-threading or multiprocessing where appropriate
- Leverage vectorization or SIMD instructions
- Reduce polling or busy waiting
- Offload tasks to GPU or dedicated hardware

---

# Memory Bottlenecks

## Description
The system lacks sufficient RAM to handle current workloads. It may fall back to using disk swap, which is significantly slower, or cause out-of-memory errors.

## Symptoms
- High memory usage or memory nearing system limit
- Excessive swapping or paging activity
- Applications crashing due to out-of-memory conditions
- Garbage collection delays in managed runtimes (e.g. Java, .NET)

## Common Causes
- Memory leaks or unreleased allocations
- Holding large in-memory datasets or caches
- Poor memory management or fragmentation
- Inefficient use of collections or data formats

## Detection Tools
- Linux: `free -m`, `vmstat`, `top`, `smem`
- Windows: Task Manager, Resource Monitor
- Heap profilers: Valgrind Massif, VisualVM, dotMemory

## Fixes
- Profile and fix memory leaks
- Use memory-efficient data types and structures
- Implement caching policies with eviction
- Process data in chunks instead of loading all at once
- Tune garbage collector settings in managed languages

---

# I/O Bottlenecks

## Description
The system is waiting on input/output operations, such as reading from or writing to disk or network. I/O operations are often the slowest relative to CPU and memory.

## Symptoms
- High disk or network wait times
- Applications stalling or blocking on file or socket reads/writes
- Disk usage at or near 100%
- Slow startup or shutdown times

## Common Causes
- Disk-heavy operations without buffering
- Reading or writing large files without batching
- Network latency or limited bandwidth
- Synchronous blocking I/O in performance-critical paths

## Detection Tools
- Linux: `iotop`, `iostat`, `dstat`, `nload`
- Windows: Resource Monitor, Performance Monitor
- Application-level: logging I/O latency and throughput

## Fixes
- Use asynchronous or non-blocking I/O
- Buffer reads and writes where possible
- Compress data to reduce I/O load
- Use faster storage (e.g. SSD instead of HDD)
- Implement connection pooling and backoff strategies for networks

---

# Summary Table

| Bottleneck Type | Key Symptom              | Key Cause                            | Fix Example                          |
|------------------|---------------------------|----------------------------------------|---------------------------------------|
| CPU              | High CPU usage            | Inefficient code, poor parallelism     | Use multithreading, optimize loops    |
| Memory           | Swapping, OOM crashes     | Leaks, large data sets, poor GC        | Fix leaks, stream data, tune GC       |
| I/O              | High latency, stalling    | Disk/network delays, blocking I/O      | Async I/O, faster disks, batching     |

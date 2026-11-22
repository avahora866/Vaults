---
tags:
  - SoftwareDesign
---

# Definitions

## Profiling
- Measures *how* a program runs
- Identifies performance issues inside the code
- Focuses on runtime behavior (CPU usage, memory allocation, function calls)

## Benchmarking
- Measures *how fast* or *how much* a program can do
- Compares performance across versions, systems, or configurations
- Uses controlled environments to produce repeatable results

---

# Profiling

## Purpose
- Find hotspots (functions consuming the most resources)
- Track memory usage, allocations, leaks
- Understand call hierarchies and execution time

## Types
| Type                 | Description                                             | Use Case                                      |
|----------------------|---------------------------------------------------------|-----------------------------------------------|
| CPU profiling        | Tracks time spent in functions or code blocks           | Identifying slow functions                    |
| Memory profiling     | Tracks allocations, usage, and leaks                    | Finding memory bloat, leaks                   |
| I/O profiling         | Tracks file, disk, and network access                  | Finding I/O wait or inefficiencies            |
| Heap profiling       | Monitors heap growth and GC behavior                   | Tuning Java/.NET runtimes                     |
| Sampling profiling   | Captures stack traces at intervals                     | Low overhead CPU usage insights               |
| Instrumentation      | Inserts hooks to log function entry/exit               | Accurate but higher overhead                  |

## Common Tools
| Language | Tools                                      |
|---------|--------------------------------------------|
| C/C++   | `gprof`, `perf`, Valgrind, Intel VTune     |
| Java    | VisualVM, YourKit, JProfiler, Java Flight Recorder |
| Python  | `cProfile`, `line_profiler`, Py-Spy        |
| .NET    | dotTrace, PerfView, dotMemory              |
| General | perfetto.dev, Chrome DevTools (JS), `strace` |

## Best Practices
- Profile real workloads, not just synthetic ones
- Start with sampling, then use instrumentation for deeper analysis
- Profile with and without optimizations
- Use multiple runs to confirm findings

---

# Benchmarking

## Purpose
- Measure performance quantitatively
- Compare versions, algorithms, or hardware
- Set performance baselines or regressions

## Key Metrics
- Throughput (e.g. requests/second, ops/sec)
- Latency (min, avg, p95, p99)
- Resource usage (CPU, memory, disk, network)
- Energy efficiency (on mobile or embedded systems)

## Types
| Type            | Description                                      | Example                                |
|-----------------|--------------------------------------------------|----------------------------------------|
| Microbenchmark  | Tests a small unit of code (e.g. sort, hash)     | `benchmark()` functions in C++, JMH in Java |
| Macrobenchmark  | Tests end-to-end system or application           | Web server under load, game loop fps   |
| Regression      | Compares performance across software versions    | Detect slowdowns after code changes    |
| Synthetic       | Uses simulated workloads                         | Disk benchmark tools, CPU burn tests   |
| Real-world      | Measures actual user-like usage patterns         | Load tests, A/B test telemetry         |

## Common Tools
| Domain     | Tools                                           |
|------------|-------------------------------------------------|
| General    | `time`, `hyperfine`, `benchstat`                |
| Java       | JMH (Java Microbenchmark Harness)               |
| C/C++      | Google Benchmark, `perf`, `time`                |
| Python     | `timeit`, `pytest-benchmark`                    |
| Web        | Lighthouse, WebPageTest                         |
| Systems    | `fio`, `iperf`, `sysbench`                      |
| Load Test  | `ab`, `wrk`, `locust`, `k6`, `JMeter`           |

## Best Practices
- Run in isolated environments (no background noise)
- Pin CPU affinity or disable frequency scaling
- Warm up before measurement to eliminate JIT bias
- Run multiple iterations and report statistics
- Use consistent input data

---

# Profiling vs Benchmarking Summary

| Aspect            | Profiling                         | Benchmarking                          |
|------------------|-----------------------------------|---------------------------------------|
| Goal             | Find *why* it's slow               | Measure *how* fast it runs            |
| Scope            | Internal program behavior          | External performance metrics          |
| Output           | Call graphs, time per function     | Timing, throughput, latency stats     |
| Use Case         | Optimization, debugging            | Comparison, validation, CI checks     |
| Overhead         | Moderate (especially with instrumentation) | Low to medium                        |

---

# Integration with Development Workflow

- Use **profiling** during development and before optimization
- Use **benchmarking** in CI to track regressions
- Set performance budgets and automate fail conditions
- Use historical results to validate hardware or deployment changes
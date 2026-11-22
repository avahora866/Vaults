---
tags:
  - WebDevelopment
---

-------------------------------------------------------------------------------
A cache is used to store something temporarily inside a computing environment (not only in frontend).

It is smaller, faster and more expensive type of [[Memory]] that can be used to increase performance of frequently or recently accessed data. Cached Data is stored temporarily in an accessible [[Storage Media]] that's local to the [[Cache Client]] and separate from the main storage .

Cache is used because bulk or main storage cant keep up with the demands of clients. Cache decreases data access times, reduces [[Latency]] and improves input/output (I/O). Because almost all application workloads depends on I/O operations, the caching process improves application performance.

# How does a cache work

When a cache client attempts to access data, it first check the cache. If the data is found there, it is referred to a **cache hit.** The percent of attempts that result in a cache hit is called the **cache hit ratio** or ratio.

If the data is not found there it is referred to as a **cache miss**. It is then pulled from the main memory and copied into the cache. How this is done, and what data is removed from the cache to make room for the new data is dependent on the Cache algorithm, Cache protocols and system policies being used

Web browsers like Safari, Firefox and Chrome use **browser caching** to improve the performance of frequently accessed webpages. When a user visits a webpage, the requested files are stored in a cache for that browser in the user's computing storage.

To retrieve a previously accessed page, the browser gets most of the files it needs from the cache instead of having it resent from the web server. This is called **read cache**. The browser can read data from the cache faster then then it can reread files from the web server.

Cache is important for several reasons:

- The use of cache reduces [[Latency]] for active data. This results in higher performance for a system or application.
- It diverts I/O to cache, reducing I/O operations to external storage and lower levels of [storage area network](https://www.techtarget.com/searchstorage/definition/storage-area-network-SAN)
- Data can remain permanently in traditional storage or external [storage arrays](https://www.techtarget.com/searchstorage/definition/array). This maintains the consistency and integrity of the data using features, such as [snapshots](https://www.techtarget.com/searchdatabackup/definition/storage-snapshot) and Replication, provided by the storage or array.
- [Flash](https://www.techtarget.com/whatis/definition/Flash) is used only for the part of the workload that will benefit from lower latency. This results in the cost-effective use of more expensive storage.
[Cache memory](https://www.techtarget.com/searchstorage/definition/cache-memory) is either included on the CPU or embedded in a chip on the system board. In newer machines, the only way to increase cache memory is to upgrade the system board and CPU to the newest generation. Older system boards may have empty slots that can be used to increase the cache memory.

## How are caches used?

Caches are used to store temporary files, using hardware and software components. An example of a hardware cache is a CPU cache. This is a small chunk of memory on the computer's processor used to store basic computer instructions that were recently used or are frequently used.

Many applications and software also have their own cache. This type of cache temporarily stores app-related data, files or instructions for fast retrieval.

Web browsers are a good example of application caching. As mentioned earlier, browsers have their own cache that store information from previous browsing sessions for use in future sessions. A user wanting to rewatch a [YouTube](https://www.techtarget.com/searchcio/definition/YouTube) video can load it faster because the browser accesses it from cache where it was saved from the previous session.

Other types of software that use caches include the following:

- operating systems, where commonly used instructions and files are stored;
- [content delivery networks](https://www.techtarget.com/searchnetworking/definition/CDN-content-delivery-network), where information is cached on the server side to deliver websites faster;
- [domain name systems](https://www.techtarget.com/searchnetworking/definition/domain-name-system), where they can be used to store information used to convert domain names to Internet Protocol addresses; and
- databases, where they can reduce latency in [database query](https://searchsqlserver.techtarget.com/definition/query)

## What are the benefits of caches?

There are several benefits of caching, including the following:

- **Performance.** Storing data in a cache allows a computer to run faster. For example, a browser cache that stores files from previous browsing sessions speeds up access to follow up sessions. A database cache speeds up data retrieval that would otherwise take a good bit of time and resources to download.
- **Offline work.** Caches also let applications function without an internet connection. Application cache provides quick access to data that has been recently accessed or is frequently used. However, cache may not provide access to all application functions.
- **Resource efficiency.** Besides speed and flexibility, caching helps physical devices conserve resources. For example, fast access to cache conserves battery power.

## What are the drawbacks of caches?

There are issues with caches, including the following:

- **Corruption.** Caches can be corrupted, making stored data no longer useful. Data corruption can cause applications such as browsers to crash or display data incorrectly.
- **Performance.** Caches are generally small stores of temporary memory. If they get too large, they can cause performance to degrade. They also can consume memory that other applications might need, negatively impacting application performance.
- **Outdated information.** Sometimes an app cache displays old or outdated information. This can cause an application glitch or return misleading information. If a website or application gets updated on the internet, using a cached version from a previous session would not reflect the update. This is not a problem for static content but is a problem for [dynamic content](https://www.techtarget.com/searchnetworking/definition/dynamic-and-static) that changes over sessions or between sessions.

## Cache algorithms

Instructions for cache maintenance are provided by cache algorithms. Some examples of cache algorithms include the following:

- **Least Frequently Used** keeps track of how often a cache entry is accessed. The item that has the lowest count gets removed first.
- **Least Recently Used** puts recently accessed items near the top of the cache. When the cache reaches its limit, the least recently accessed items are removed.
- **Most Recently Used** removes the most recently accessed items first. This approach is best when older items are more likely to be used.

## Cache policies

[Various caching policies](https://www.techtarget.com/searchstorage/answer/Comparing-write-through-write-back-and-write-around-caching) determine how the cache operates. Then include the following:

- **Write-around cache** writes operations to storage, skipping the cache. This prevents the cache from being flooded when there are large amounts of write I/O. The disadvantage to this approach is that data isn't cached unless it's read from storage. As a result, the read operation is slower because the data hasn't been cached.
- **Write-through cache** writes data to cache and storage. The advantage of [write-through](https://www.techtarget.com/whatis/definition/write-through) cache is that newly written data is always cached, so it can be read quickly. A drawback is that write operations aren't considered complete until the data is written to both the cache and [primary storage](https://www.techtarget.com/searchstorage/definition/primary-storage). This can introduce latency into write operations.
- **Write-back cache** is like write-through in that all the write operations are directed to the cache. But with [write-back](https://www.techtarget.com/whatis/definition/write-back) cache, the write operation is considered complete after the data is cached. Once that happens, the data is copied from the cache to storage.

With this approach, both read and write operations have low latency. The downside is that, depending on what caching mechanism is used, the data remains vulnerable to loss until it's committed to storage.

## What does clearing a cache do and how often should it be done?

Clearing the cache frees [memory space](https://www.techtarget.com/searchstorage/definition/virtual-memory) on a device. A browser cache uses memory to store files downloaded directly from the web. Clearing it can solve user issues, such as the following;
- A full cache memory can cause applications to crash or not load properly.
- Old caches can contain outdated information and files, causing webpages to not load or load incorrectly. Clearing them can get rid of outdated information.
- Browser caches also contain saved personal information, such as passwords. Clearing them can protect the user.
- Most browser caches can be cleared by going to settings.

A cache should be cleared periodically, but not daily. Clearing the cache too often is not a good use of resources because of these issues:
- the user loses the benefit of quick file access;
- caches delete some files on their own and don't need this sort of maintenance; and
- the computer will cache new files and fill the space up again.

### Types of caches

Caching is used for many purposes. The various cache methods include the following:

- **Cache memory** is RAM that a microprocessor can access faster than it can access regular RAM. It is often tied directly to the CPU and is used to cache instructions that are accessed a lot. A RAM cache is faster than a disk-based one, but cache memory is faster than a RAM cache because it's close to the CPU.
- See how RAM and cache memory compare in terms of use, speed, capacity and cost.
- **Cache server**, sometimes called a proxy cache, is a dedicated network server or service. [Cache servers](https://www.techtarget.com/whatis/definition/cache-server) save webpages or other internet content locally.
- **CPU cache** is a bit of memory placed on the CPU. This memory operates at the speed of the CPU rather than at the system [bus](https://www.techtarget.com/searchstorage/definition/bus) speed and is much faster than [RAM](https://www.techtarget.com/searchstorage/definition/RAM-random-access-memory).
- **Disk cache** holds recently read data and, sometimes, adjacent data areas that are likely to be accessed soon. Some [disk caches](https://www.techtarget.com/searchstorage/definition/disk-cache) cache data based on how frequently it's read. Frequently read storage blocks are referred to
- **Flash cache,** also known as [solid-state drive caching](https://www.techtarget.com/searchstorage/definition/SSD-caching), uses [NAND flash memory](https://www.techtarget.com/searchstorage/definition/NAND-flash-memory) chips to temporarily store data. Flash cache fulfils data requests faster than if the cache were on a traditional hard disk drive or part of the backing store.
- **Persistent cache** is ****storage capacity where data isn't lost in the case of a system [reboot](https://www.techtarget.com/whatis/definition/reboot-warm-boot-cold-boot) or crash. A battery backup is used to protect data or data is flushed to a battery-backed [dynamic RAM](https://www.techtarget.com/searchstorage/definition/DRAM) as extra protection against data loss.
- **RAM cache** usually includes permanent memory embedded on the motherboard and memory modules that can be installed in dedicated slots or attachment locations. The mainboard bus provides access to this memory. CPU cache memory is between 10 to 100 times faster than RAM, requiring only a few [nanoseconds](https://www.techtarget.com/whatis/definition/nanosecond-ns-or-nsec) to respond to a CPU request. RAM cache has a faster response time than [magnetic media](https://www.techtarget.com/searchstorage/definition/Persistent-storage), which delivers I/O at rates in milliseconds.
- **Translation lookaside buffer,** also called [TLB](https://www.techtarget.com/whatis/definition/translation-look-aside-buffer-TLB), is a memory cache that stores recent translations of virtual memory to [physical addresses](https://www.techtarget.com/whatis/definition/physical-address) and speeds up virtual memory operations.
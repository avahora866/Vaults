---
tags:
  - Architecture
---
A distributed system, also known as distributed computing, is a system with multiple components located on different machines that communicate and coordinate actions in order to appear as a single coherent system to the end-user.

The machines that are a part of a distributed system may be computers, physical servers, [virtual machines](https://www.stackpath.com/products/virtual-machines), [containers](https://www.stackpath.com/products/containers), or any other node that can connect to the network, have local memory, and communicate by passing messages.

There are two general ways that distributed systems function:

1. Each machine works toward a common goal, and the end-user views results as one cohesive unit.
2. Each machine has its own end-user, and the distributed system facilitates sharing resources or communication services.

Although distributed systems can sometimes be obscure, they usually have three primary characteristics: all components run concurrently, there is no global clock, and all components fail independently of each other.

# Benefits and challenges of distributed systems

There are three reasons that teams generally decide to [implement distributed systems](https://www.oreilly.com/ideas/distributed-systems-a-quick-and-simple-definition):

- **Horizontal Scalability:** Since computing happens independently on each node, it is easy and generally inexpensive to add additional nodes and functionality as necessary.
- **Reliability:** Most distributed systems are fault-tolerant, as they can be made up of hundreds of nodes that work together. The system generally doesn’t experience any disruptions if a single machine fails.
- **Performance:** Distributed systems are extremely efficient because work loads can be broken up and sent to multiple machines.

However, distributed systems are not without challenges. Complex architectural design, construction, and debugging processes that are required to create an effective distributed system can be overwhelming.

Three more challenges you may encounter include:

- **Scheduling:** A distributed system has to decide which jobs need to run, when they should run, and where they should run. Schedulers ultimately have limitations, leading to underutilized hardware and unpredictable [runtimes](https://blog.stackpath.com/runtime/).
- **Latency:** The more widely your system is distributed, the more latency you can experience with communications. This often leads to teams making trade-offs between availability, consistency, and latency.
- **Observability:** Gathering, processing, presenting, and monitoring hardware usage metrics for large clusters is a significant challenge.

# How a Distributed System Works

Hardware and software architectures are used to maintain a distributed system. Everything must be interconnected—CPUs via the network and processes via the communication system.

## Types of distributed systems

Distributed systems generally fall into one of four different basic architecture models:

1. **Client-server:** Clients contact the server for data, then format it and display it to the end-user. The end-user can also make a change from the client-side and commit it back to the server to make it permanent.
2. **Three-tier:** Information about the client is stored in a middle tier rather than on the client to simplify application deployment. This architecture model is most common for [web applications](https://blog.stackpath.com/web-application/).
3. **_n_tier:** Generally used when an application or server needs to forward requests to additional enterprise services on the network.
4. **Peer-to-peer:** There are no additional machines used to provide services or manage resources. Responsibilities are uniformly distributed among machines in the system, known as peers, which can serve as either client or server.
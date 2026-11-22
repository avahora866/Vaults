---
tags:
  - Theory
---
# Latency
Latency is the **time it takes for data to pass from one point on a network to another**. Suppose Server A in New York sends a data packet to Server B in London. Server A sends the packet at **04:38:00.000 GMT** and Server B receives it at **04:38:00.145 GMT**. The amount of latency on this path is the difference between these two times: **0.145 seconds or 145 milliseconds**.

Most often, latency is measured **between a user's device (the "client" device) and a data center**. This measurement helps developers understand how quickly a web page or application will load for users.

Although data on the Internet travels at the speed of light, the effects of distance and delays caused by Internet infrastructure equipment mean that latency can never be eliminated completely. It can and should, however, be minimized. **A high amount of latency results in poor website performance**, negatively affects [[SEO]], and can induce users to leave the site or application altogether.

## Causes of Internet Latency
One of the principal causes of network latency is **distance**, specifically the distance **between client devices making requests and the servers responding to those requests**. If a website is hosted in a data center in Columbus, Ohio, it will receive requests fairly quickly from users in Cincinnati (about 100 miles away), likely within 5-10 milliseconds. On the other hand, requests from users in Los Angeles (about 2,200 miles away) will take longer to arrive, closer to 40-50 milliseconds.

An increase of a few milliseconds may not seem like a lot, but this is **compounded by all the back-and-forth communication necessary for the client and server to establish a connection**, the total size and load time of the page, and any problems with the network equipment the data passes through along the way. **The amount of time it takes for a response to reach a client device after a client request is known as round trip time (RTT)**. RTT is equal to _double the amount of latency_, since data has to travel in both directions — there and back again.

Data traversing the Internet usually has to cross not just one, but multiple networks. The more networks that an HTTP response needs to pass through, the more opportunities there are for delays. For example, as data packets cross between networks, they go through Internet Exchange Points (IXPs). There, routers have to process and route the data packets, and at times routers may need to break them up into smaller packets, all of which adds a few milliseconds to RTT.

## How can latency be reduced
- **Reduce physical distance**
    - Use [[Content Delivery Networks (CDNs)]] to serve data closer to users.
    - Choose servers/data centers geographically near your location.
- **Improve routing**
    - Use optimized DNS (e.g., Google DNS, Cloudflare DNS) for faster paths.
    - Connect via ISPs with better peering and fewer hops.
- **Upgrade transmission medium**
    - Use fiber instead of DSL/copper.
    - Prefer wired Ethernet over Wi-Fi.
- **Reduce congestion**
    - Increase available bandwidth to prevent queuing delays.
    - Apply QoS to prioritize latency-sensitive traffic (gaming, VoIP).
- **Improve wireless connections**
    - Move closer to access points.
    - Use 5 GHz or 6 GHz bands for less interference.
    - Avoid overcrowded Wi-Fi channels.
- **Optimize protocols and applications**
    - Use UDP where possible (gaming, streaming, VoIP).
    - Enable TCP optimizations like window scaling.
    - Cache frequently accessed data locally.
- **Reduce server-side delays**
    - Use faster servers with optimized software.
    - Balance load across multiple servers.

# Bandwidth
**Bandwidth** is the **maximum amount of data that can pass through the network at any given time.** Usually measured in bits per second (bps), kilobits per second (kbps), megabits per second (Mbps), or gigabits per second (Gbps).

## What affects bandwidth
- **Physical medium**
    - Copper cables (Ethernet, coaxial) have lower capacity than fiber-optic cables.
    - Wireless channels are constrained by radio frequency spectrum and interference.        
- **Network hardware**
    - Routers, switches, and network interface cards (NICs) have maximum supported speeds.
    - Older hardware can bottleneck connections (e.g., 100 Mbps switch on a 1 Gbps line).
- **Signal quality and noise**
    - Electrical interference, poor cabling, or long distances reduce effective bandwidth.
    - Wireless bandwidth is degraded by obstacles, distance from the access point, and competing devices.
- **Network congestion**
    - More users on the same connection or channel lowers available bandwidth per user.
    - Shared infrastructure (like ISP backbones) can throttle effective speeds at peak times.
- **Protocols and overhead**
    - TCP/IP, encryption, error correction, and packet headers consume part of the available bandwidth.
    - High-latency connections (e.g., satellite internet) can reduce effective throughput even if nominal bandwidth is high.
- **Service provider limits**
    - ISPs may cap speeds, apply data throttling, or enforce quality of service (QoS) rules that reduce bandwidth

## How to increase bandwidth
- **Upgrade physical medium**
    - Switch from copper to fiber for higher speeds.
    - Use higher-grade Ethernet cables (Cat6/6a/7 vs. Cat5e).
- **Upgrade network hardware**
    - Replace old routers, switches, and NICs with devices that support faster standards (e.g., Wi-Fi 6/6E, 10 Gbps Ethernet).
- **Improve signal quality**
    - Shorten cable runs, reduce interference, or install repeaters/extenders.
    - For Wi-Fi, place access points in optimal locations and use less congested frequency bands (5 GHz, 6 GHz).
- **Reduce congestion**
    - Segment networks (e.g., VLANs) or add more access points.
    - Apply traffic management so heavy users/apps don’t saturate the link.
- **Use compression and efficient protocols**
    - Enable data compression where possible.
    - Optimize applications to send fewer, larger packets instead of many small ones.
- **Increase service capacity**
    - Upgrade to a higher-bandwidth ISP plan.
    - For organizations, lease more lines or peer directly with providers.

# Throughput
Throughput is the **average amount of data that actually passes through over a given period of time**. **Throughput is not necessarily equivalent to bandwidth**, because it is **affected by latency** and other factors. Measured in bits per second (bps). It’s the _real-world performance_ you experience, whereas bandwidth is the _maximum possible capacity_.

## What affects throughput
- **Available bandwidth**
    - Throughput can’t exceed the maximum bandwidth of the channel.
- **Network congestion**
    - Competing traffic reduces throughput for each user.
    - Packet collisions and retransmissions lower efficiency.
- **Latency**
    - High latency (satellite internet, long-distance connections) slows acknowledgment cycles in TCP, reducing effective throughput.
- **Packet loss**
    - Lost or dropped packets must be retransmitted, which reduces throughput.
    - Common causes: interference (Wi-Fi), poor cabling, or overloaded routers.
- **Protocol overhead**
    - TCP/IP headers, encryption, error correction, and acknowledgments reduce usable data rate.
    - Some protocols (e.g., UDP) have less overhead than TCP but sacrifice reliability.
- **Server or client performance**
    - Slow CPUs, disks, or application bottlenecks can reduce how much data is processed per second, independent of bandwidth.
- **Quality of Service (QoS) or throttling**
    - ISPs or network admins may intentionally prioritize or limit certain traffic.
## What increases throughput
- **Increase available bandwidth**
    - Upgrade ISP plan or switch to higher-capacity physical media.
- **Reduce congestion**
    - Add more access points, balance loads, or schedule heavy transfers during off-peak times.
    - Use QoS to prioritize important traffic.
- **Minimize latency and packet loss**
    - Optimize routing paths (e.g., use a CDN for web content).
    - Improve Wi-Fi signal strength with better placement, antennas, or wired connections.
    - Use higher-quality cables and networking equipment.
- **Optimize protocols**
    - Use faster transport methods (e.g., UDP for streaming/gaming).
    - Enable TCP acceleration techniques like window scaling.
    - Use compression to reduce payload sizes.
- **Optimize applications and endpoints**
    - Ensure servers and clients aren’t CPU, memory, or disk bottlenecked.
    - Tune applications for fewer, larger transfers instead of many small ones.
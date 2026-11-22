---
tags:
  - SystemDesign
---
Long polling is a technique used to **keep a client updated with server data without constantly sending repeated requests**. It’s a way to simulate “real-time” updates over HTTP when WebSockets or server-sent events aren’t used.

How it works:

1. The client sends a request to the server asking for new data.
2. If the server has new data, it responds immediately.
3. If there is no new data, the server **holds the request open** until:

   * New data becomes available, or
   * A timeout occurs.
4. Once the client receives a response, it immediately sends another request, repeating the cycle.

Pros:

* Works over standard HTTP/HTTPS.
* Simple to implement compared to WebSockets.
* Reduces unnecessary polling compared to short-interval polling.

Cons:

* Each request ties up server resources while waiting.
* Still slightly less efficient than WebSockets for high-frequency updates.
* Complexity grows with many concurrent clients.

Use cases:

* Chat applications where messages need to arrive in near real-time.
* Notifications systems.
* Updating dashboards when data changes frequently.

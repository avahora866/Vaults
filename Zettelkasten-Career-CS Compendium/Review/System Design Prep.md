- [[Latency, Throughput and Bandwidth|Latency]]: Latency is the **time it takes for data to pass from one point on a network to another**
- [[Latency, Throughput and Bandwidth|Throughput]]
- [[Websocket]]
- [[API Gateway]]
- [[Load Balancer]]
- [[Content Delivery Networks (CDNs)]]
- [[Data Access Patterns]]
- [[ReadWrite ratio]]
- [[Functional vs Non-Functional Requirements]]
- [[Database partitioning (sharding, replication)|Database Replicas]]. Seems we only read from replicas and write to the primary

# Youtube
- [[Object Store]]
- [[SQL vs NoSQL performance]] 
- [[Encoding]] and [[Compression]]
- [[Redis|In memory cache]]:
	- LRU Cache
- [[_Network Protocols]]
---
# Ticketmaster
- [[CAP theorem and BASE vs ACID]]
- ~~Stripe payment~~
- *Database cron job*
- *In memeory cache (redi) attaches to a database??*
	- TTL
- [[Search optimised DB]]
	- Elasticsearch
- [[Change Data Capture]] - how to get data from primary db into elasticsearch (also cache dbs like redis?)
- [[Long Polling]]: when you make a request and keep the request open as you expect a response. keep it open for aboput a minute 
- Persistent connections ([[Websocket]], sse)

# Uber
- [[HTTP]]
- API Design = Dont put user id inside body, tell interviewer it will be in the header as either JWT or session id
- Make sure to note why you are using a separate microservice. Is it more computational heavy, is asynchronous.
- Microservices can call each other: through REST endpoitns or gRPC
- **stopped halway through**

# Dropbox/GDrive
- Mention latency in non-functional requirments
- Non-functional requirments should not jsut include stuff regarding the 'iliteis' but should also include other qualities o fhte program like for dropdbox (file max size of 50gb)
- **Remeber to only get 3 functional requirments**
- The concept of chunking files
- The concept of fingerprinting
- pre-signed url
- trust-but-verify pattern
- Compression on various file types 
- long pulling: when you make a request and keep the request open as you expect a response. keep it open for aboput a minute 
# Twitter
- Load balancer routing alogos: round robin, least connection, ip hash, etc
- What layer does the load balancer operate Layer 4 and Layer 7
- Choose an algo for the load balancer 
- Why is blob storage good for files media etc
- Rate limiting is usually for writes 
- Graph DB
- Monitoring tools  - health checks Prometheus + Grafana
- **Load balancer**: reliability, scalability, health checks, routing across instances.
- **API gateway**: request management, authentication, rate limiting, logging, protocol translation.



- SQL ACID Consistency vs NOSQL Availability
- Distributed locking 
- SAGA patterns 

---

# Framework
1. Understand problem and establish design scope
	1. Ask questions to clarify requirements
		- Why we are building the system, who the users are and what features we need to build
		- Goal is to find the features we are building in priority order. Focus on the top 3 features
	- Ask questions to clarify the non-functional requirements: for system design interviews its mainly scale and performance
2. Propose high level design and get buy in
	- Start with the APIs
		- Unless specified otherwise we should follow the rest API convention
		- Define the API's input and outputs
	- High level design diagram
		- Blueprint of the overall design to refer back to 
		- For many designs we start with a load balancer or an API gateway
		- Followed by the services that will satisfy the feature requirements
		- Followed by the data storage layer 
		- Whilst doing the high level design maintain a list of talking points for later: database scaling, high concurrency, failure scenarios
	- Data model schema
		- Data access patterns
		- Read/Write ratio
		- Choose the right database 
3. Design deep dive
	- Work with the interview on where to discuss next
	- This is where the non-functional requirements come into play
	- When we find a problem that we can discuss we should come up with multiple solutions and discuss the various tradeoffs
		- First articulate the problem
		- Come up with two solutions
		- Discuss tradeoff of solutions (use numbers)
		- Pick a solution
		- **In a typical interview we should only have the time to dive deep into the top 2 or 3 issues**
4. wrap up
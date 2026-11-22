---
tags:
  - "#SECONDARY_BRANCH"
---
# Routing
When a packet arrives at a router, the router has a fraction of a second to answer one question: "Which cable do I push this out of?"

To do this, it consults its Routing Table.

Think of the Routing Table as the Departures Board at a train station.
- It does not list every single house in the world (that would be impossible).
- It lists Networks (Cities/Districts) and the Next Hop (which track the train leaves on).

The Logic Flow:
- Packet Arrives: The router looks at the Destination IP (e.g., 50.1.2.3).
- The Lookup: It scans the Routing Table for a network that matches that IP.
- The Decision: It finds the entry for 50.0.0.0 (The generic network for that IP).
- The Action: The table says: "For 50.0.0.0, send traffic to Neighbor Router B."

## How the Table gets Populated
The table is not magic; it must be learned. There are three ways a router learns a route:
1. Directly Connected Routes
    - If a cable is plugged directly into the router, it automatically knows that network exists.
2. Static Routing (Manual)
    - Concept: The Network Administrator acts as the dictator.
    - Action: I type a command: "If you see a packet for Google, send it to Router X."
    - Pro: Precise control.
    - Con: If Router X breaks, the traffic stops. The router is too stupid to find a new path on its own.
3. Dynamic Routing (The Protocols)
    - Concept: The routers talk to each other.
    - Action: Router A tells Router B: "Hey, I am connected to the Internet." Router B tells Router C: "Hey, I can get to the Internet through Router A."
    - Examples: OSPF, EIGRP, BGP.
    - The Magic: If a cable breaks, the routers instantly "gossip" about the failure, update their tables, and reroute traffic around the break. This is how the packet "finds" a new path.

## The Metric (The Cost)
Often, a router will know two ways to get to the same destination.
- Path A: Through the high-speed fiber optic cable.
- Path B: Through a slow satellite link.
How does it decide? It uses a Metric (a score).
- OSPF uses Bandwidth (Speed). It prefers the fast road.
- RIP (an old protocol) uses Hop Count. It prefers the path with the fewest routers, even if that path is slower.

# Subnetting
Subnetting is simply the logic of drawing borders. 

The Subnet Mask tells the computer where the "Network" ends and the "Host" begins. It acts like a stencil or a filter.
- 255: Means "Match this number perfectly." (The Network slice).
- 0: Means "This part can vary." (The User slice).

Example: The Standard Home Network
- IP: 192.168.1.5
- Mask: 255.255.255.0

The Subnet Mask is a Local Decision Tool. It is used Before the packet leaves your computer.

,1st Octet,2nd Octet,3rd Octet,4th Octet
IP,192,168,1,5
Mask,255,255,255,0
Meaning,Network,Network,Network,Host

1. The first three numbers (192.168.1) are locked. This is the Network ID. Every device in your house must have these three numbers to talk to each other.
2. The last number (.5) is the Host ID. This acts as the specific "Name" of the device.

If you bring in a device with 192.168.1.50, it matches the first three numbers. It is "In the Network." If you bring in a device with 192.168.5.50, the third number does not match. It is a stranger.

## CIDR Notation (The Slash /)
Writing 255.255.255.0 is tedious. Network engineers use a shorthand called CIDR (Classless Inter-Domain Routing).

We count the number of "1s" in the binary version of the mask. 255.255.255.0 has 24 bits turned on (8+8+8). So we write: 192.168.1.5/24.
- /24 means "The first 24 bits (3 numbers) are the Network."
- /16 means "The first 16 bits (2 numbers) are the Network."
- /8 means "The first 8 bits (1 number) is the Network."

## Step by Step Walkthrough
### Step 1: The "Local vs. Remote" Check
You (Computer A) want to send data to 192.168.2.10. Your IP is 192.168.1.10. Your Mask is 255.255.255.0 (which tells you the first 3 numbers define your "tribe").
Your computer runs a quick calculation (called a Bitwise AND operation):

1. Check My Network: It looks at your IP and your Mask.
    - Result: "I am in network 192.168.1."
2. Check Target Network: It looks at the Destination IP and applies your Mask.
    - Result: "The target is in network 192.168.2."
3. The Comparison:
    - "Does 192.168.1 match 192.168.2?" -> NO.

### Step 2: The Decision
Because the networks do not match, your computer knows: "This destination is foreign. I cannot just hand this packet to them directly."
- Action: Your computer packages the data and sends it to the Default Gateway (The Router). It essentially says, "Router, this is not my problem. You handle it."

If they HAD matched: If you were sending to 192.168.1.50:
- The math would show a match.
- Action: Your computer would shout locally (using ARP), "Hey 192.168.1.50, here is a file directly for you." The Router is ignored.

Note: Your computer (Source) does not know the Destination's subnet mask. It cannot see inside the other computer's settings.
---

## Subnetting a Subnet
What if there is a subnet `192.168.1.0 /25`

A /25 means we have borrowed 1 extra bit from the host side. Instead of 0-255 (256 addresses), we cut the pizza in half.
Subnet 1: 192.168.1.0 to 192.168.1.127
Subnet 2: 192.168.1.128 to 192.168.1.255

The Cheat Sheet (Powers of 2): Block Sizes
/24 = 256 IPs (Standard Class C)
/25 = 128 IPs
/26 = 64 IPs
/27 = 32 IPs
/28 = 16 IPs
/29 = 8 IPs
/30 = 4 IPs (Used for links between routers)

# Dynamic Routing Protocols
These are languages routers use to chat with each other to get the map of the internet so they can travel. Note these protocols are for getting the map not for calculating the route
There are two main types:

## Distance Vector (The Gossip)
- Protocol: RIP (Routing Information Protocol).
- Logic: "I don't know the full map. I only know what my neighbors tell me."
- Analogy: You want to get to the airport. You ask your neighbor. He says, "I think it's 5 miles that way." You don't know if he's right, you just trust him.
- Metric: Hop Count (How many routers do I cross?).
- Flaw: It is slow to update and can create "loops" where data spins in a circle forever.

## Link State (The Sat-Nav)
- Protocol: OSPF (Open Shortest Path First).
- Logic: "I want to see the ENTIRE map myself."
- Action: Every router sends a "Hello" packet to the entire network saying, "I am here, and I am connected to Router B."
- Result: Every router builds a complete, 3D map of the network in its memory. Then, it runs a math formula (Dijkstra's Algorithm) to calculate the absolute fastest path.
- Metric: Cost (Based on Speed/Bandwidth).
---
tags:
  - "#TRUNK"
---
# Distinction between network and internet
## Network
A network is a general term for any collection of two or more interconnected devices (computers, routers, printers, etc..) that can exchange data and share resources
- Scope: Limited and localized.
- Scale: Small to large (e.g., a few devices in a home, a whole office building, or a corporate structure spanning a few cities).
- Administration: Typically managed by a single entity (a person, a company, or an organization).

Examples:
- LAN (Local Area Network): Your home Wi-Fi network or an office floor.
- WAN (Wide Area Network): A corporation's network linking its headquarters and several branch offices.

## Internet
The Internet is a specific, massive, and global network of networks. It is an interconnected system that links all the smaller private, public, academic, and business networks worldwide
- Scope: Global and public.
- Scale: Vast, connecting billions of devices.
- Protocol: It relies on a universal set of standards, primarily the Internet Protocol suite (TCP/IP), to ensure that all these diverse networks can communicate with each other.

Relationship to Networks: Your personal network (LAN) must connect to an Internet Service Provider (ISP), which then connects to other ISPs and the main global infrastructure to access the Internet

# TCP/IP Model and OSI Model
The TCP/IP model is more practical and implementation-driven. It was designed after the protocols of the internet (TCP, IP).
- Directly models the Internet.
- It was created from observing how the Internet actually works.
- Each layer corresponds to real protocols that the Internet uses:
    - Internet layer → IP addresses, routing
    - Transport layer → TCP/UDP for reliable or fast delivery
    - Application layer → HTTP, DNS, FTP, etc.

TCP/IP models the real protocols and software that the internet uses, not just an abstract framework. So, TCP/IP is practical: it’s a reflection of the Internet’s architecture.

The OSI model is a conceptual framework. It doesn’t describe actual protocols or software, but it models how data should flow from one computer to another in a network, broken into logical steps.
- Does not model the Internet specifically.
- It’s a conceptual framework for all types of networks, not just the Internet.
- Its seven layers are an idealized view of how data communication could be structured.
- Some OSI layers (Session, Presentation) don’t have a direct equivalent in the Internet—they’re “folded” into TCP/IP’s application layer.

So, OSI is theoretical: it models networking principles in general, providing a guide for designing protocols.

## OSI Model
The OSI model is a theoretical model that describes how data moves through a network. It separates communication into seven distinct layers, each with its own responsibilities:

1. Physical Layer
    - Handles the physical transmission of raw bits over a medium (cables, fiber, wireless signals).
    - Deals with voltages, light pulses, radio waves, and connectors.
    - Defines hardware specifications, signaling, and data rates.
2. Data Link Layer
    - Ensures reliable transfer of data across a single network segment.
    - Packages data into frames, handles error detection/correction, and manages MAC addresses.
    - Controls access to the physical medium, preventing collisions and coordinating transmission.
3. Network Layer
    - Responsible for moving data across multiple networks.
    - Uses logical addresses to route data from source to destination.
    - Handles routing and forwarding decisions.
    - Protocols at this layer define how packets travel across interconnected networks.
4. Transport Layer
    - Provides end-to-end communication between applications on different devices.
    - Ensures data arrives reliably, in order, and without duplication (depending on protocol).
    - Can manage flow control and error recovery.
5. Session Layer
    - Manages sessions or connections between applications.
    - Controls when sessions start, end, or resume.
    - Coordinates dialog between applications on different hosts.
6. Presentation Layer
    - Translates, formats, or transforms data for the application.
    - Handles encryption, compression, and data representation.
    - Ensures that data sent by one system can be understood by another.
7. Application Layer
    - Provides network services directly to end-user applications.
    - Interfaces with software programs, enabling them to communicate over the network.
    - Examples: file transfer, email, network management, and web browsing.

### Key Idea
The OSI model is a blueprint for designing and understanding networks. Each layer has a clearly defined role, so you can think about networking as a series of stages: from sending raw bits, to packaging them, to routing them across networks, to delivering them to applications in the right format.

It’s not tied to any specific technology or protocol—it’s meant to be universal.

## TCP/IP Model
The TCP/IP model is a practical model for network communication, designed to describe how computers communicate over the Internet. It has four layers, each with specific responsibilities:
1. Link Layer
    - Handles communication between a device and the local network it’s connected to.
    - Deals with physical connections, network interface hardware, and local addressing.
    - Responsible for sending raw data over cables, Wi-Fi, or other physical media.
    - Examples: Ethernet, Wi-Fi, ARP.
2. Internet Layer
    - Responsible for delivering packets from one host to another across multiple networks.
    - Uses logical addressing (IP addresses) and routing to find the path from source to destination.
    - Ensures that data can travel across networks it doesn’t directly know about.
    - Examples: IP (IPv4/IPv6), ICMP (for network diagnostics like ping).
3. Transport Layer
    - Ensures end-to-end communication between applications on two devices.
    - Provides reliability, ordering, and error-checking depending on the protocol.
    - Examples: TCP (reliable, connection-oriented), UDP (unreliable, fast, connectionless).
4. Application Layer
    - Provides protocols and services directly used by software applications.
    - Handles things like web browsing, email, file transfers, and domain name resolution.
    - Examples: HTTP, HTTPS, SMTP, DNS, FTP.

### How it works together
1. Applications generate data and hand it to the transport layer.
2. Transport layer breaks it into segments, adds headers for reliability, and passes them to the Internet layer.
3. Internet layer adds IP addresses, determines routes, and sends packets toward the destination.
4. Link layer sends the packets over the local network to the next device, which repeats the process until they reach the destination.
5. At the destination, the data moves up the layers in reverse order to reach the application.

### The concept of layers inside TCP/IP
The layers in the TCP/IP model don’t each represent a separate physical connection between computers. Instead, they represent different levels of responsibility in the process of sending data from one computer to another.
Imaging a connection between two computers to be a string. The layers are sections of the string vertically at the beginning and end of the connection not horizontally.

#### How Layers Work in a Connection
1. At the sender (Computer A):
    - The application creates data and hands it down to the Transport layer, which segments it and adds headers.
    - The Internet layer adds IP addressing and routing info.
    - The Link layer adds local network information (like MAC addresses) and actually transmits the bits onto the physical network.
2. Across the network:
    - The data is transmitted as packets over the physical network.
    - Routers and switches forward packets using information from the Internet and Link layers, but they don’t implement all the higher-level layers—they mostly operate at the Internet and Link layers.
3. At the receiver (Computer B):
    - The Link layer receives the bits and strips its headers.
    - The Internet layer checks the destination IP and removes its headers.
    - The Transport layer reassembles segments, checks for errors, and delivers them to the Application layer, where the program can use the data.

##### Key Point
The layers are conceptually present only at the endpoints (the computers sending and receiving data).
In the network itself (like routers or switches), only some layers are “used” to forward data—they don’t need to reconstruct the full message.
So the full layered process happens at the start and end of the connection, while the network in between mainly handles transport and routing.

---
The TCP/IP model is essentially a layered approach to moving data from one computer to another across the Internet, specifying what each layer is responsible for without tying itself to a theoretical model.

# The Internet Protocol (IP) Address
An IP address serves two simultaneous functions: identification (who you are) and location (where you are).
- The Network Portion: Tells the internet roughly where to send the data.
- The Host Portion: Identifies the specific machine on that network.

Currently there are two versions:
- IPv4 (The Old Standard): Uses 32 bits. It looks like `192.168.1.1`. Hoever, the primary limitation for this format is it only allows for approximately 4.3 billion unique addresses. We have mathematically run out of these addresses due to the explosion of mobile devices.
- IPv6 (The New Standard): Uses 128 bits. It looks like `2001:0db8:85a3:0000:0000:8a2e:0370:7334`. This provides $3.4 \times 10^{38}$ addresses. To visualize this: if every atom on the surface of the Earth were a computer, we would still have enough IPv6 addresses to give one to each of them.

## Public vs Private IP'S
## Private IP's
These are used only inside your Local Area Network (LAN)—your home or office.
- These are used inside your home or office. Your router assigns these to your phone, laptop, and smart fridge (e.g., `192.168.x.x`). The outside internet cannot see these. Your router acts as the gatekeeper, translating your private requests into a public one (a process called NAT).
- The Rule: These addresses are non-routable on the internet. If you try to send a packet to a Private IP across the internet, the routers will drop it immediately. They do not exist on the global map.
- The Look: They almost always start with:
    - 192.168.x.x (Most home networks)
    - 10.x.x.x (Large corporate networks)
- The Magic: You can have a device with the IP 192.168.1.5 in your house. I can also have a device with 192.168.1.5 in my house. This is fine because they never meet.

## Public IP's
These are used on the Wide Area Network (WAN)—the internet.
- This is the address the outside world sees. It is assigned to your router by your ISP (Internet Service Provider). It is like the address written on your mailbox.
- The Rule: These must be globally unique. There can only be one device in the entire world with the address 8.8.8.8 (which belongs to Google).
- Assignment: Your Internet Service Provider (ISP) owns a pool of these expensive public addresses. They lend one to your house.

## The Connector: NAT (Network Address Translation)
- Your Router is the Doorman.
- Sending Out: You (Apt 101 / Private IP) want to request google.com. You hand the request to your Router.
- Translation: The Router (Doorman) takes your request, crosses out "Apt 101," and stamps the return address as "500 Science Way" (Public IP). He then throws it out to the internet.
- Receiving: Google replies to "500 Science Way."
- Translation Back: The Router receives the reply. He looks at his clipboard and remembers: "Ah, Apt 101 asked for this 2 seconds ago." He hands the data back to you.

# The Domain Name System (DNS)
Computers operate strictly in numbers (IPs). Humans operate in language (Domain Names like google.com). DNS is the bridge between the two.

It is not just a flat list; it is a rigid hierarchy. When you type www.example.com, your computer does not magically know the IP. It hunts for it in a specific order:

1. The Recursive Resolver: Your computer asks your ISP: "Do you know the IP for example.com?"
2. The Root Server (.): If the ISP doesn't know, it asks the Root Server. The Root doesn't know the specific IP, but it knows who handles .com.
3. The TLD Server (.com): The ISP then asks the .com server. This server knows who handles example.com specifically (the "Authoritative" server).
4. The Authoritative Name Server: Finally, this server says: "Yes, example.com is located at 93.184.216.34."

# Packet Switching
When you download a large file—say, a 1 GB movie—the internet does not open a single, continuous pipe and pour the movie through like water. If we did that, a single line break would ruin the entire transfer, and one user could clog the whole network.

Instead, we use Packet Switching.

The Core Concept: Data is chopped into tiny, manageable chunks called packets. Each packet is wrapped with a "header" containing the source IP, destination IP, and a sequence number (e.g., "Packet 1 of 1000").

These packets are then fired out onto the network individually.
1. Fragmentation: The file is disassembled at the source.
2. Independent Routing: This is crucial. Packet #1 might travel through a cable in France. Packet #2 might travel through a satellite link over the Atlantic. Packet #3 might go through a fiber line in New York. They behave like cars navigating traffic; they take whichever road is fastest at that exact second.
3. Reassembly: When they arrive at your computer, they might arrive out of order (Packet #10 might arrive before Packet #2). Your computer reads the sequence numbers, puts them back in order, and reconstructs the movie.

A typical packet has two parts:
- Header (Control Information): Contains crucial details like the source and destination IP addresses, the packet's sequence number (for reassembly), and other control information. Think of this as the digital envelope with the mailing address.
- Payload (User Data): This is the actual data being transmitted (a segment of the file, text, image, etc.).

# TCP vs UDP
There are two different protocols for actually shipping the data:

## TCP (Transmission Control Protocol) — "The Perfectionist"
TCP is obsessed with reliability. It guarantees that every single byte arrives, and arrives in the correct order.
1. The Handshake: Before sending data, the sender and receiver introduce themselves ("3-Way Handshake").
2. Numbering: Every packet is numbered.
3. Acknowledgement (ACK): When the receiver gets a packet, it sends a receipt back to the sender ("I got Packet #1").
4. Retransmission: If the sender does not get a receipt for Packet #2 (because of the shark), it resends it automatically.

Use Case: Web browsing (http), Emails (smtp), File transfers (ftp). Precision is more important than speed. If you are downloading a program, a single missing bit could corrupt the entire file.

## UDP (User Datagram Protocol) — "The Speed Demon"
UDP does not care about reliability. It cares about speed. It fires packets at the target as fast as possible and never looks back.
1. No Handshake: It just starts talking.
2. No Receipts: It does not care if you received the packet.
3. No Retransmission: If a packet is lost, it is gone forever.

Use Case: Live Video Streaming, Online Gaming, Voice over IP (VoIP): If you are watching a live soccer match and a packet containing a single frame of video is lost, you do not want the video to freeze while the server resends that frame. By the time it arrives, the match has moved on. You just accept a tiny glitch in the video and keep going.

# The Application Layer: HTTP and HTTPS
This is the language the computers speak once the connection is made.

## HTTP (HyperText Transfer Protocol)
This is the standerd way data is transmitted across networks. It consists of headers, body, params etc.
The computer on the other side expects this kind of format and so it can translate it.

## HTTPS
HTTP: It is sent in "plaintext." If you send a password over HTTP, anyone sitting in the coffee shop with a packet sniffer can read it as it flies through the air. It is like writing your password on a postcard.

HTTPS (Secure): The 'S' stands for Security (specifically, SSL/TLS encryption).
- Before any data is sent, your browser and the server agree on a secret code.
- The data is scrambled. Password123 becomes &%#@9d7A!.
- Even if a hacker steals the packets, they only see garbage.

# The Application Layer: FTP
File Transfer Protocol (FTP) is a standard network protocol used to transfer files between a client and a server on a computer network

Unlike HTTP, which typically uses a single connection, FTP is unique because it establishes two separate connections:
- Control Channel: Transmits commands (like "log in" or "change directory") and responses.
- Data Channel: Handles the actual transfer of file data.

1. The Handshake (Control Channel - Port 21): The client connects to the server's Port 21. This line stays open for the entire session. It is used only for sending commands (like USER, PASS, STOR to upload, RETR to download) and receiving status codes (like 200 OK).
2. The Request: You ask to download a file. The client sends a command like RETR myfile.txt over the Control Channel.
3. The Data Channel Opens (Port 20 or Random): This is the strange part. The server and client negotiate a brand new connection just for this one file.
    - The Control Channel says: "Okay, I'm ready to send. Open the data line."
    - The Data Channel connects.
4. The Transfer: The actual file content flows through the Data Channel. The Control Channel goes idle (or sends "keep-alive" packets) while this happens.
5. Teardown: Once the file is finished, the Data Channel is cut and closed immediately. The Control Channel remains open, waiting for your next command.

When you use standard FTP, your username, password, and the file data itself are sent across the network in plain text.

To fix this, we need encryption. There are two primary ways to achieve this, but they work very differently:
1. FTPS (FTP over SSL/TLS): This wraps the standard FTP protocol in an encrypted layer, similar to how HTTPS works for websites. It still uses the complex two-channel system (Control and Data ports) we mentioned earlier.
2. SFTP (SSH File Transfer Protocol): Despite the name, this isn't FTP at all! It is a completely different protocol built on top of SSH (Secure Shell). It typically uses just one single connection for everything.


# Network Ports
We have got data from Computer A to Computer B. But there is one final problem. Your computer is doing many things at once. You are browsing a website (Chrome), listening to music (Spotify), and checking email (Outlook) all at the same time. When a packet arrives at your computer's IP address, how does your computer know which application gets the data? Does the music data go to your spreadsheet? Does the email go to your video player?

This is solved by Ports.

If the IP Address is the Street Address of a building, the Port is the Room Number.
- IP Address: Ensures the packet reaches the correct machine.
- Port Number: Ensures the packet reaches the correct software application running on that machine.

Every packet that leaves or enters your computer has a port number stamped on it (inside the TCP/UDP header).
We write this as `IP_Address:Port`:`192.168.1.5:80`

## The "Well-Known" Ports
To make the internet work, we agreed on standard room numbers for common services. If you start a web server, it automatically listens to Room 80.

Port Number,Protocol,Use Case
80,HTTP,Unsecured Web Browsing
443,HTTPS,Secured Web Browsing (The Padlock)
25,SMTP,Sending Emails
22,SSH,Securely controlling a server remotely
53,DNS,Looking up domain names

When you type google.com into your browser, your computer automatically assumes you mean google.com:443 because you are using a web browser.

# Conclusion
Scenario: You open your laptop (Private IP 10.0.0.5) and type www.bank.com.
1. DNS: Your computer asks the DNS server "What is the IP for bank.com?" and gets 1.2.3.4.
2. Encapsulation: Your browser creates a request:
    - Data: "Get me the homepage" (HTTP).
    - Port: "Send this to Port 443" (HTTPS/TCP).
    - IP: "Send this to 1.2.3.4" (IP).
3. Transmission: The packet leaves your laptop, goes to your router (NAT), flies across the internet (Packet Switching), and arrives at the Bank's server.
4. The Arrival:
    - The Bank's server sees the IP and knows "This is for me."
    - It looks at the Port (443) and knows "This is a web request, hand it to the Web Server software."
    - It unwraps the Data and replies.
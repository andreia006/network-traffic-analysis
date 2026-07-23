# DNS Protocol Documentation

| Category | Value |
|----------|-------|
| Protocol | ICMP |
| OSI Layer | Network (Layer 3) |
| Primary Purpose | Communication Diagnostics |
| Tools Used | Wireshark, Windows Command Prompt |

---

# Objective

The purpose of this lab is to better understand the core functionality of the ICMP protocol and to understand what it is. This is m first exposure to the protocol, so it is also an opportunity to analyze the protocol in action and generate traffic to study from.

---

# Background

### What is ICMP?:
ICMP, standing for Internet Control Message Protocol is a protocol primarily used for communication diagnostics, report delivery errors and share operational information. Simplistically, it is a tool that gives out information about a commucation between two networks and facilitates network maintanence with this crucial information it provides.

### How does a DNS work?:
This protocol as mentioned provides tools for diagnostics. The most common ones being *ping* and *tracert (traceroute on Mac/Linux)*.


**- Ping:** This command checks whether a source is alive, available or reachable. For example, "ping google.com" we are sending a ping echo request to google.com. google.com should reply with an echo response to notify it is reachable.


**- Tracert:** This command displays all of the router hops data has to go through to reach it's target. 

### Extra Information:
This protocol does not require TCP unlike DNS because we do not need to transport information with this protocol. This protocol simply checks the status of a connection. It is designed to be lightweight.

---

# Lab Environment

Operating System: Windows 11

Network: Home Wi-Fi Network

Software: Wireshark, Command Prompt

Traffic Generated: google.com, youtube.com, github.com

---

# Procedure

### 1. Wireshark Setup:
Select a network to begin. For this lab I will be using my home wi-fi network.


### 2. Command Prompt Setup:
After selecting a network, I opened my command prompt to prepare to generate traffic using *ping*.


### 3. Generated Network Traffic:
I pinged google.com, youtube.com and github.com once per each in the command prompt as I ran the capture on Wireshark.

### 4. Display filter:
I applied the ICMP filter to view the traffic I generated. But as I did so, I stumbled upon a ICMPv6 filter. I researched this and discovered that ICMPv6 is an advanced version of ICMPv4, the protocol being observed in this lab. While the ICMPv4 version in an optional debugging tool, ICMPv6 is crucial to basic network functionality. Without it, local communication would break. It does not support fragmentation and enables v6 devices to generate their own IP address.


In regards to the ICMPv4 traffic, I was able to locate my generated traffic. I identified a Source and Destiation by IP, as well as a request and response. Typically 4 requests and 4 responses are initiated in a ping test and it is reflected in the filter. Upon analysing the Internet Control Message Protocol Tab, I was also able to see the type of echo. A (8) echo is a request while a (0) is a response.


While the time to live in all three ping tests was nowhere close to death, I noticed that compared to google.com and youtube.com github's response time is a lot slower. Youtube and Google border around 7-8 milliseconds while Github responded with a total of 31 milliseconds.

### 5. Analyze with Tracert:
In the command prompt, I had typed tracert google.com. There, I was able to see in real time all the hops between routers until data reached it's target. It displays the IPs of each router, but inside the journey the message "Request time out" appeared several times. I researched this as well, and 3 possible reasons for this behaviour is possible:


**1 - Router is Configured To Hide:** Many enterprises and ISPs configure theur routers to drop ICMP traffic. This is a security feature to prevent revealing internal topology of a network to possible attackers.


**2 - Severe Traffic Congestion:** A router is programmed to protect it's CPU during heavy load. One of these rules include prioritizing user traffic. Meaning if it has already received too many ICMP requests or it is busy it will begin to silently drop ICMP requests.


**3 - Network Break:** This happens when the packet has reached a dead end. A cable is cut, the device is completely powered down our the routing table is misconfigured. The destination is not accessible at this point and will continue to show "Request Time Out" until the 30-hop maximum.


In this case, I was able to reach google.com, but the lather reason could be a valid reason why some requests times out.

### 6 - Generating Errors:

---

# Display Filter

```text
icmp
```

---

# Packet Analysis

### - What happened?
I observed the flow of traffic with the DNS protocol filter to closely monitor DND behaviour in action. I was able to find the traffic I generated on purpose. I specifically focused on the Doamin Name System and the Standard Query. I looked for the response in which the IP address was provided and didn't realize a request can have several responses.
I was able to see the communicated address in response, and Youtube specifically had different IP addresses it responded the query with (Answers RRs: 8, so 8 different addresses). After researching why, I came to the conclusion it could simply because it uses made IPs for load balancing purposes.

I mistaked the dst and src as the responses I was looking for. It is important to make the distinction that src and dst simply point out *who* is talking in the conversation but not an indicator towards what exactly is being communicated.

After analyzing a bit more of unrelated traffic under the same filter I picked up on the pattern in how the communication is happening and how responses show up.

---

# Screenshots

### Screenshot 1

<img width="1157" height="212" alt="Screenshot 2026-07-20 201740" src="https://github.com/user-attachments/assets/260a8ed5-1128-4623-982d-8edb35d8b801" />

Explanation:

This is what I saw when I am referring to with the 8 Answer RRs. In the query response, you can see it responding with 8 IPS, the text cutting off. Here is also when I realized multiple querys and responses we sent. A response was sent before the actual DNS IP was sent.

---

# What I Learned

In this lab I learned and gained a deeper understanding of the DNS protocol. While I had a general idea of what it was, practicing it hands-on on Wireshark helped me demistify missconceptions I was not aware I was carrying about the protocol. For example, DST and SRC and very different things from the actual response from a DNS phonebook. As well as why a query answer can come attached with more than one IP.

From a security lense, I also gained knowledge about IPs themselves. I learned the importance of IPs in communicating with other networks.

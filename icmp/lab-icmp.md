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


**3 - Network Break:** This happens when the packet has reached a dead end. A cable is cut, the device is completely powered down our the routing table is misconfigured. The destination is not accessible at this point and will continue to show "Request Time Out" until the 30-hop maximum. Or it will displa an error "Destination net Unreachable".


In this case, I was able to reach google.com, but the lather reason could be a valid reason why some requests times out.

### 6 - Generating Errors:
In this case, I was curious about generating the Network Break error. What I did was tracert to an invalid IP address. This time I captured the error in wireshark. The same error I observed in the command prompt is the same I observed in wireshark. The TTL expired in transit as well as the network being unreachable.

---

# Display Filter

```text
icmp
```

```text
icmpv6
```
---

# Packet Analysis

### - What happened?
To summarize my observations, there were clear patterns in the manner this protocol is used as well as expected behaviour depending on what is generated.


4 Responses, 4 Replies - This is common occurance with initiating echo requests and responses. Even if a domain/network is only pinged once, 4 responses and replies are not uncommon to see.


Types of Echos - 8 for a request, 0 for a response.


Sequence Number - The Sequence Number has to and matches bother the request and response. they must have the same sequence number.


---

# Screenshots

### Screenshot 1

<img width="767" height="365" alt="Screenshot 2026-07-23 175118" src="https://github.com/user-attachments/assets/f6b3d678-3a6f-4ece-bf31-fbc45bdc7151" />

Explanation:

This is the successful ping in the command prompt. Here we can also observe 4 responses from google.com and the average response speed. For GitHub it was an average of 31ms.

### Screenshot 2

<img width="962" height="182" alt="Screenshot 2026-07-23 180208" src="https://github.com/user-attachments/assets/2e55974c-268c-4093-9012-204ed0035112" />

Explanation:

This screenshot shows the back and forth reply and request in Wireshark with the ICMP filter. We can observe the sequence number in this screenshot. This was also traffic from google.com.

### Screenshot 3

<img width="845" height="247" alt="Screenshot 2026-07-23 185745" src="https://github.com/user-attachments/assets/5be6f636-be44-44c8-a1b9-7f0bed659291" />

Explanation:

This screenshot displays the intentional error displayed in Wireshark with the invalid IP address. We can observed the TTL expiration and the final Netowrk Unreachable Error.

---

# What I Learned

In this lab, I went from not knowing what the protocol was beforehand to feeling quite comfortable with it's definition and what it does. I learned it can be a very useful tool for troubleshooting. It demonstrated how echo requests verify connectivity as well as ping utility to verify whether a target is reachable and traceroute to show the data's journey from the computer to it's target through hops within routers.


# TCP Protocol Documentation

| Category | Value |
|----------|-------|
| Protocol | TCP |
| OSI Layer | Transport (Layer 4) |
| Primary Purpose | Ensures data arrives accurately, in order, and without missing pieces to it's target. |
| Tools Used | Wireshark |

---

# Objective

The objective of this lab is to understand as a whole what the TCP protocol is, what it's responsible for and what it looks like in action. Generation of TCP traffic will be done in order to also better analyze *The Three-Way Handshake*.

---

# Background

### What is the TCP protocol?:
The TCP protocol, standing for Transmission Control Protocol is a protocol working at the transport layer that ensures all data delivery is ordered and reliable as well as error checked. For this to happen, a TCP connection must be established first before data transfer. This is where the **Three-Way Handshake* comes into play.

**- Three-Way Handshake:** This is a TCP connection is initiated. The sender will send a packet with a **SYN** flag active. This tells the receiver "I want to connect" or "sync". The receiver will then send a packet with a **SYN** and **ACK** flag back to the sender. This tells the sender "I have received your request, and would also like to connect". Finally, the sender sends a packet with a **ACK** flag, which reads as an acknowledgement or "Alright, let's start". After this process a connection has been established and the payload transmission begins.

### How does the TCP protocol work?:
Immediately after a connection is established, data transmission begins. The transmission itself can face various challenges depending on various conditions. However the protocol acts as a manager to manage all of these circumstances with three main mechanisms:

**- Tracking and Ordering:** TCP breaks larger files into smaller pieces called *segments* to fulfill network requirements. This process organizes segments as well before sending. However layer 3 protocols are still capable of shrinking segments into **fragments** if they still appear too big, though often avoided.
Every byte of data then receives a **Sequence Number**. If a sender wanted to send a 3000 byte file, and due to the network's size limits the file must be broken into 1000 byte chunks- the sequence number starts where it begins processing. So the SN for the first segment would be 1 (1-1000), the second one would be 1001(1001-2000), and the third would be numbered 2001(2001-3000). After this, the receiver must send an **ACK** number which tells the sender what byte they expect next.


**- Flow Control:** The receiver has a temporary storage area called a buffer.
In every ACK packet the receiver sends, a *window size* value is included. This number indicates to the sender how many bytes of data it can handle at the moment. This value is not constant and can shrink if the receiver's app gets slow or other circumstances. It can most certainly also hit 0, which halts the sender from transmitting anything else until the receiver clears their buffer.

### Extra Information:
While the Three-Way Handshake is happening, at each phase a random Initial Sequence Number (ISN) is generated and used at each phase. At the first phase, the sender generates an ISN set to x. At the second stage, the receiver sets the number to X + 1 as acknowledgement and generates it's own ISN called y. At the third stage, the sender also sets the receiver's ISN to Y + 1 in acknowledgement.

---

# Lab Environment

Operating System: Windows 11

Network: Home Wi-Fi Network

Software: Wireshark

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
tcp
```

---

# Packet Analysis

### - What happened?
...


---

# Screenshots

### Screenshot 1

...

Explanation:

...

### Screenshot 2

...

Explanation:

...

### Screenshot 3

...

Explanation:

...

---

# What I Learned

...

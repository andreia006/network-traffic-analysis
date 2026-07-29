
# TCP Protocol Documentation

| Category | Value |
|----------|-------|
| Protocol | TCP |
| OSI Layer | Transport (Layer 4) |
| Primary Purpose | Ensures data arrives accurately, in order, and without missing pieces to it's target. |
| Tools Used | Wireshark |

---

# Objective

The objective of this lab is to understand as a whole what the TCP protocol is, what it's responsible for and what it looks like in action. Generation of TCP traffic will be done in order to also better analyze *The Three-Way Handshake*, it's innerworkings and possibly when things go wrong.

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


**- Congestion Control:** The sender is not aware of how overloaded a router can be. So the transmission starts with very small amounts of data. If receiver ACKs are returned quickly, TCP assumes the network is clear and can double the amount of data being transmitted rapidly. However, if an ACK response isn't returned back to the sender for the next byte in a set amount of time (A packet is dropped), TCP instantly cuts down the transmission speed to relieve network pressure. From there, the slow start process can restart as needed.

### Extra Information:
While the Three-Way Handshake is happening, at each phase a random Initial Sequence Number (ISN) is generated and used at each phase. At the first phase, the sender generates an ISN set to x. At the second stage, the receiver sets the number to X + 1 as acknowledgement and generates it's own ISN called y. At the third stage, the sender also sets the receiver's ISN to Y + 1 in acknowledgement.


While TCP manages data transmission, things can still go wrong. If packet #2 arrives before packet #1, the receiver may hold packet #2 in memory but refuse to acknowledge it until the necessary packet arrives.
Packet loss can also happen, which is in the event of a packet vanishing, the receiver will keep asking for the missing packet by sending duplicate ACKs. The sender is able to notice this and perform a fast transfer for the missing piece.

---

# Lab Environment

Operating System: Windows 11

Network: Home Wi-Fi Network

Software: Wireshark

Traffic Generated: microsoft.com, wikipedia.com, github.com

---

# Procedure

### 1. Wireshark Setup:
Select a network to begin. For this lab I will be using my home wi-fi network.

### 2. Generated Network Traffic:
I opened the chrome browser and captured my visits to microsoft.com, wikipedia.com and github.com onto wireshark.

### 3. Display filter:
I appled the TCP filter at first to check on my findings. There were many things I didn't understand at first upon using the filter. Each flag appeared to be doubled but the three way handshake pattern was there. After some research there are a multitude of reasons why. Such as Wireshark itself, multiple connections to the same website for CSS, Java code, etc.. After more research I was able to find a suitable filter that allowed me to cleanly see TCP connection without other interference. I right clicked one of the packets went into Follow->TCP Stream. It set the tcp.stream eq 0 filter.

### 4. Analyze The Traffic:
I did notice new flags as well after the connection was established. **[PSH]** and **[FIN]**. Right after the last ACK from my device, my device sent a **[PSH,ACK]** flag to the receiver. I researched the meaning of this flag and it means to tell the website to "Send this information immediately, do not hold it in the buffer". Or in this specific scenario it is saying "Here is my request, push it to the web server app right now." which can be seen as an HTTP GET request. The server/website sends an **[ACK]** flag. Acknowledging the request and that it received the request safely.
Then **[FIN, ACK]** is the beginning of the end of the connection. My computer initiated his. Which means, "I have no more data to send, I want to close my outbound channel". The website acknowledges this with an **[ACK]** flag; "Message received. I am stopping the tracking of the upload stream.". Then it sends the same **[FIN, ACK]** flag also stating it has nothing else to send. The final acknowldegment sent from my device ends the connection.

This the commencement of the connection AND closing combined is now a Four-Way Handshake.

The interaction of the TCP, was very straightforward with the filter. But looking at the statistics some things mentioned previously as visible as well. The Window Size, Sequence, Length and MSS(Maximum Segment Size). The last two bits of information create a system on how data is transmitted at a deeper level. The Maximum Segment Size is established in the beginning of the connection. This maximum size prevents **fragmentation** as mentioned earlier. It as a typical limit agreed upon in which a packet bigger than that amount is to never be transmitted and needs to be turned into segments first. Length is exactly how large the size of the data actually is. For my capture, it was fairly small in the [PSH,ACK] request, coming around 2 length.

All of these elements work together into a system. MSS sets a roof over the max size of a segment but how fast it's being transmitted still affects Window size and buffer overload. The sender can send as many packets under the MSS limit but as well as within the receiver's window. Once the window is closed, an ACK is necessary to keep going with a new window size. The Sequence statistic, as mentioned earlier also tracks at which byte it's tracking from. Sequence 1 and Len 2 means that it's tracking data from byte 1 to 2.

Noteably when an MSS is being negotiated in the 3 way handshake- the lowest number wins. My device offered 1460 which is the standard. but the website went with 1452 due to network tunneling and encapsulation.

---

# Display Filter

```text
tcp
```

```text
tcp.stream eq 0
```
---

# Packet Analysis

### - What happened?
As I explained in my traffic analysis I successfully analyzed the procedures in which a TCP three way handshake happens and even how a four way handshake happens. Importantly, about the payload itself, Wireshark displays crucial information about the payload's length, and even transmission byte tracking information. Noteably, while I applied the correct filter in the end, the DNS protocol showed up along the TCP filter. This allowed me to also see in real time how protocols function in junction with others. A DNS request was initiated after data was transmitted, and after the receiver's acknowledgement a DNS response was given.


---

# Screenshots

### Screenshot 1

<img width="1472" height="408" alt="Screenshot 2026-07-29 160811(3)" src="https://github.com/user-attachments/assets/4d9f8de0-415c-4623-b13a-3629edf0ed38" />

Explanation:

This screenshot shows the filter I used after right clicking a package. It shows the clear TCP path, as well as all the information I made discoveries about and explained in detail about what each field meant.

### Screenshot 2

<img width="497" height="180" alt="Screenshot 2026-07-29 164141" src="https://github.com/user-attachments/assets/c8a8d0e8-f7d2-47ef-bb35-671dba32cf1d" />

Explanation:

This screenshot is an observation about the size of the payload and segment data.

---

# What I Learned

I learned about one of the most important protocols used everyday. It was definitely not as intuitive as DNS and ICMP, and definitely has deeper innerworkings than the ones I analyzed on previous protocols. I learned about the three-way handshake, communication flags, payload information crucial for transmission, transmission rules and how it is closed in the end with a four-way handshake.

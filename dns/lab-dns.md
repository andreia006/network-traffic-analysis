# DNS Protocol Documentation

| Category | Value |
|----------|-------|
| Protocol | DNS |
| OSI Layer | Application (Layer 7) |
| Transport Protocol | UDP (53), TCP (53 for some cases) |
| Primary Purpose | Resolves domain names into computer legible IP addresses |
| Tools Used | Wireshark, Windows Command Prompt |

---

# Objective

The objective of this lab is to better understand and observe how DNS traffic appears on Wireshark, and it's role in converting domain names into computer legible IP addresses. This lab also aims to better understand what exactly DNS is and the transport protocols associated with it. With this, I will develop a deeper understanding into how IP addresses are used during everyday web browsing.

---

# Background

### What is a DNS?:
DNS standing for Domain Name Search is a protocol that works a little bit like a phonebook for the internet. Computers cannot understand domain names such as "google.com" or "youtube.com". Computers communicate using *IP Addresses*. But having the user remember IP addresses every time they need to access a website can become difficult. The DNS protocol bridges between this gap.
It acts as a translator from domain names. Translating it into IP addresses.

### How does a DNS work?:
The user's computer may send a quick packet called a *DNS query* to a local DNS server. This server is this "phonebook" we refer to as an analogy. Containing millions of demains and their IP addresses. The packet sent to this server is asking for the IP address of the domain the computer wants to access. The server then finds and replies with the IP address of the domain.

### Extra Information:
A computer OS usually comes with it's own miniature and temporary copy of the phonebook, called a *DNS cache*. When the user accesses a website such as "google.com", the computer only looks it up from the internet once, then stores the reference (IP address) to that domain in it's cache. So when the user *flushes their DNS* it clears the local DNS cache. Forcing the lookup of domains from the internet once again. This is very good to use when the cache is holding onto an outdated version of a website.

---

# Lab Environment

Operating System: Windows 11

Network: Home Wi-Fi Network

Software: Wireshark, Command Prompt

Traffic Generated: google.com, youtube.com

---

# Procedure

### 1. Wireshark Setup:
A common mistake when opening up wireshark is attempting to capture without selecting a network. There are many connections one could analyze such as a bluetooth network connection, but for this lab demonstration I am using my home network.


### 2. Packet Capture:
After selecting a network, I began capturing for a good amount of time. I was able to see information of inbound and outbound IP addresses, port information, TTL, and bytes captured.


### 3. Generated Network Traffic:
I visited google.com and youtube.com as part of the traffic I manually generate on my machine several times. However, previously opened tabs are a curisoity of mine to find as well when  applying the filter.

### 4. Display filter:
I applied the DNS filter successfully and was able to see many websites under the info section. Under this section I was able to see query to and query responses and domain names. I am able to trace the "google.com" I visited several times, as well as "youtube.com". Curiously, other requests and responses from other domains such as "Steam" and "ChatGPT" were observed in the capture despite me not using the services actively. This highlights the fact that a computer, while appearing to, is never actually idle. Background services can periodically do DNS lookups for website changes, syncronize data, or verify server availability.

### 5. Saved packet capture:
For screenshotting and referencing what I have found, I saved the capture.

---

# Display Filter

```text
dns
```

Replace with whatever protocol you're analyzing.

---

# Packet Analysis

### - What happened?
I observed the flow of traffic with the DNS protocol filter to closely monitor DND behaviour in action. I was able to find the traffic I generated on purpose. I specifically focused on the Doamin Name System and the Standard Query. I looked for the response in which the IP address was provided and didn't realize a request can have several responses.
I was able to see the communicated address in response, and Youtube specifically had different IP addresses it responded the query with (Answers RRs: 8, so 8 different addresses). After researching why, I came to the conclusion it could simply because it uses made IPs for load balancing purposes.

I mistaked the dst and src as the responses I was looking for. It is important to make the distinction that src and dst simply point out *who* is talking in the conversation but not an indicator towards what exactly is being communicated.

After analyzing a bit more of unrelated traffic under the same filter I picked up on the pattern in how the communication is happening and how responses show up.

---

# Screenshots

### Example 1

...

Explanation:

...

---

### Example 2

...

Explanation:

...

---

# What I Learned

Summarize what you learned from the lab.

Focus on concepts rather than memorization.

---

# Interview Takeaway

Imagine an interviewer asks:

"Can you explain DNS?"

Write the answer you'd confidently give.

Keep it short (3–5 sentences).

---

# Future Exploration

Things I'd like to learn next:

- Example
- Example
- Example

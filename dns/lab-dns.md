# Protocol Name

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

## What is a DNS?:
DNS standing for Domain Name Search is a protocol that works a little bit like a phonebook for the internet. Computers cannot understand domain names such as "google.com" or "youtube.com". Computers communicate using *IP Addresses*. But having the user remember IP addresses every time they need to access a website can become difficult. The DNS protocol bridges between this gap.
It acts as a translator from domain names. Translating it into IP addresses.

## How does a DNS work?:
The user's computer may send a quick packet called a *DNS query* to a local DNS server. This server is this "phonebook" we refer to as an analogy. Containing millions of demains and their IP addresses. The packet sent to this server is asking for the IP address of the domain the computer wants to access. The server then finds and replies with the IP address of the domain.

## Extra Information:
A computer OS usually comes with it's own miniature and temporary copy of the phonebook, called a *DNS cache*. When the user accesses a website such as "google.com", the computer only looks it up from the internet once, then stores the reference (IP address) to that domain in it's cache. So when the user *flushes their DNS* it clears the local DNS cache. Forcing the lookup of domains from the internet once again. This is very good to use when the cache is holding onto an outdated version of a website.

---

# Lab Environment

Operating System: Windows 11

Network: Home Wi-Fi Network

Software: Wireshark, Command Prompt

Traffic Generated: TBA

---

# Procedure

Document every step.

Example:

1. Opened Wireshark.
2. Selected the active Wi-Fi adapter.
3. Started packet capture.
4. Generated network traffic.
5. Applied the display filter.
6. Analyzed packets.
7. Saved packet capture.

---

# Display Filter

```text
dns
```

Replace with whatever protocol you're analyzing.

---

# Packet Analysis

Explain what packets you observed.

Questions to answer:

- What happened?
- Which devices communicated?
- What information stood out?
- Were there any repeated patterns?

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

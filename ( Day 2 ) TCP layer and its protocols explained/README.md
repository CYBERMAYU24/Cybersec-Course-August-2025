

---

# TCP/IP Model & Protocols — Full Layer-wise Guide

The **TCP/IP model** has **4 layers** (sometimes shown as 5 with “Physical” included from OSI).
Each layer has its own set of protocols that serve specific purposes.

---

## **1) Application Layer**

**Purpose:**
Provides services directly to the end user or applications. Encodes/decodes data into a form understood by both sending and receiving applications.

**Key Protocols:**

| Protocol       | Purpose                                    | Notes/Examples                                                                                     |
| -------------- | ------------------------------------------ | -------------------------------------------------------------------------------------------------- |
| **HTTP/HTTPS** | Web browsing, API communication            | HTTPS uses TLS for encryption; HTTP/2, HTTP/3 (QUIC) for speed.                                    |
| **FTP**        | File transfer                              | Uses TCP ports 20 & 21; can operate in active or passive mode.                                     |
| **SFTP**       | Secure File Transfer over SSH              | Runs over port 22; encrypted.                                                                      |
| **SMTP**       | Send email                                 | TCP port 25; often with STARTTLS for encryption.                                                   |
| **IMAP/POP3**  | Receive/read email                         | IMAP (port 143) allows sync; POP3 (port 110) downloads. Secure variants: IMAPS (993), POP3S (995). |
| **DNS**        | Name resolution (domain ↔ IP)              | Uses UDP (port 53) primarily, TCP for large responses or zone transfers.                           |
| **DHCP**       | Dynamic host configuration (IP assignment) | UDP ports 67/68; automates IP, mask, gateway, DNS assignment.                                      |
| **SSH**        | Secure remote login                        | TCP port 22; encrypted shell sessions.                                                             |
| **Telnet**     | Remote login (insecure)                    | Plain-text; largely obsolete.                                                                      |
| **SNMP**       | Network monitoring & management            | UDP ports 161/162; GET/SET/Trap messages.                                                          |
| **NTP**        | Network time synchronization               | UDP port 123; precise clock sync.                                                                  |
| **MQTT**       | Lightweight IoT messaging                  | TCP/UDP; designed for low-bandwidth devices.                                                       |
| **RTP/RTCP**   | Real-time audio/video streaming            | Often over UDP; used in VoIP, video calls.                                                         |
| **SIP**        | Session initiation (VoIP calls)            | Sets up/modifies/terminates multimedia sessions.                                                   |

---

## **2) Transport Layer**

**Purpose:**
Ensures end-to-end communication, reliability, and correct sequencing.

**Key Protocols:**

| Protocol                                        | Purpose                                   | Features                                                                |
| ----------------------------------------------- | ----------------------------------------- | ----------------------------------------------------------------------- |
| **TCP** (Transmission Control Protocol)         | Reliable, ordered, error-checked delivery | Connection-oriented, 3-way handshake, flow control, congestion control. |
| **UDP** (User Datagram Protocol)                | Fast, connectionless data transfer        | No reliability or ordering; used for DNS, streaming, gaming.            |
| **SCTP** (Stream Control Transmission Protocol) | Reliable, multi-stream delivery           | Multi-homing, avoids head-of-line blocking.                             |
| **DCCP** (Datagram Congestion Control Protocol) | Congestion-controlled, unreliable         | Used in streaming/media scenarios.                                      |
| **QUIC**                                        | Modern transport over UDP                 | Multiplexed streams, 0-RTT TLS 1.3, basis of HTTP/3.                    |

---

## **3) Internet Layer**

**Purpose:**
Responsible for logical addressing, routing, and path determination.

**Key Protocols:**

| Protocol          | Purpose                               | Notes                                                   |
| ----------------- | ------------------------------------- | ------------------------------------------------------- |
| **IPv4**          | 32-bit addressing and routing         | Supports fragmentation; address exhaustion issues.      |
| **IPv6**          | 128-bit addressing, modernized header | Eliminates NAT, supports SLAAC, more efficient routing. |
| **ICMP**          | Error reporting, diagnostics          | E.g., “ping” uses ICMP Echo Request/Reply.              |
| **ICMPv6**        | IPv6 control messages                 | Includes Neighbor Discovery Protocol (NDP).             |
| **ARP**           | IPv4 address ↔ MAC mapping            | Works within a LAN; replaced by NDP in IPv6.            |
| **RARP**          | Reverse mapping (MAC → IP)            | Mostly obsolete; replaced by DHCP/BOOTP.                |
| **IGMP**          | IPv4 multicast group management       | Hosts join/leave multicast groups.                      |
| **EIGRP** (Cisco) | Dynamic routing                       | Advanced Distance Vector (Cisco proprietary).           |
| **OSPF**          | Link-state routing within an AS       | Finds shortest paths using Dijkstra’s algorithm.        |
| **BGP**           | Path-vector routing between ASes      | Used on the global Internet to exchange routes.         |

---

## **4) Network Access / Link Layer**

**Purpose:**
Moves data between physical devices over the same network segment. Handles framing, MAC addressing, and physical media access.

**Key Protocols & Standards:**

| Protocol/Standard                 | Purpose                        | Notes                                                     |
| --------------------------------- | ------------------------------ | --------------------------------------------------------- |
| **Ethernet (IEEE 802.3)**         | Wired LAN communication        | Uses MAC addresses; full/half-duplex.                     |
| **Wi-Fi (IEEE 802.11)**           | Wireless LAN                   | WPA2/WPA3 for encryption.                                 |
| **PPP** (Point-to-Point Protocol) | Serial link encapsulation      | Used in DSL, VPN tunnels.                                 |
| **HDLC**                          | High-level data link control   | WAN connections, framing.                                 |
| **Frame Relay**                   | Packet-switched WAN            | Mostly obsolete.                                          |
| **ATM**                           | Asynchronous Transfer Mode     | Fixed 53-byte cells; used in legacy telecom.              |
| **MAC** (Media Access Control)    | Addressing at link layer       | Unique 48-bit (or 64-bit) IDs.                            |
| **VLAN** (IEEE 802.1Q)            | Virtual LAN tagging            | Adds 4-byte VLAN tag to Ethernet frames.                  |
| **MPLS**                          | Multi-Protocol Label Switching | Works between L2 and L3 (“Layer 2.5”); speeds up routing. |

---

## **Diagram — TCP/IP Layers vs OSI Model**

```
TCP/IP Model        OSI Model          Examples of Protocols
-------------------------------------------------------------
Application         Application       HTTP, FTP, DNS, SMTP
                    Presentation      (integrated in Application in TCP/IP)
                    Session           
Transport           Transport         TCP, UDP, SCTP, QUIC
Internet            Network           IPv4, IPv6, ICMP, OSPF, BGP
Network Access      Data Link         Ethernet, PPP, Wi-Fi, MPLS
                    Physical          Copper, Fiber, Radio
```

---

## **Summary Table**

| Layer           | Main Function                | Protocol Examples                |
| --------------- | ---------------------------- | -------------------------------- |
| **Application** | User-facing services         | HTTP, FTP, DNS, SMTP, SNMP, DHCP |
| **Transport**   | End-to-end delivery          | TCP, UDP, SCTP, QUIC             |
| **Internet**    | Logical addressing & routing | IPv4, IPv6, ICMP, ARP, OSPF, BGP |
| **Link**        | Local delivery over media    | Ethernet, Wi-Fi, PPP, VLAN       |

---

## 2) IPv4 vs IPv6 — which is better & why?

| Feature       | **IPv4**                  | **IPv6**                                 | Why IPv6 is generally “better”                                                               |
| ------------- | ------------------------- | ---------------------------------------- | -------------------------------------------------------------------------------------------- |
| Address size  | 32-bit (\~4.3B addresses) | **128-bit** (\~3.4×10³⁸)                 | Vast space → no exhaustion; true end-to-end possible.                                        |
| Header size   | 20–60 bytes (variable)    | **40 bytes fixed** (+ extension headers) | Simpler/faster routing; options moved to extensions.                                         |
| NAT           | Common (RFC1918 + NAT)    | Not required; end-to-end                 | NAT removal reduces complexity/latency; some security myths about NAT—use firewalls instead. |
| Auto-config   | DHCPv4                    | **SLAAC** + DHCPv6                       | Plug-and-play with Router Advertisements.                                                    |
| Broadcast     | Yes                       | **No broadcast**; uses **multicast**     | Less noise; neighbor discovery via multicast.                                                |
| Fragmentation | Routers may fragment      | **Only the sender fragments**            | Relies on Path MTU Discovery; fewer router burdens.                                          |
| Checksums     | IPv4 header has checksum  | **No header checksum**                   | Routers do less work (L4/L2 cover integrity).                                                |
| Security      | Add-on (IPsec optional)   | IPsec designed-in (now “recommended”)    | Ubiquitous support; still policy-driven.                                                     |
| QoS           | TOS/DSCP                  | **Traffic Class + Flow Label**           | Better flow identification.                                                                  |

**Bottom line:** **IPv6** is technically cleaner, scales globally, avoids NAT side-effects, and simplifies ops. In 2025, most stacks and ISPs support dual stack. **Best practice:** run **dual-stack**; prefer IPv6 when reachable (often equal or faster). IPv4 remains necessary for legacy-only destinations.

---

## 3) Subnetting & Subnet Masks

### 3.1 IPv4 subnetting basics

* **CIDR** notation: `A.B.C.D/PrefixLen` (e.g., `192.168.1.0/24`).
* A **subnet mask** marks **network bits** as 1 and **host bits** as 0.
  Example: `/24` → mask `255.255.255.0` (24 ones).
* **Formulas (classic IPv4):**

  * Subnets created by borrowing **s** bits: `#subnets = 2^s`.
  * Hosts per subnet: `2^h − 2` (reserve network & broadcast).
    *Exception:* **/31** point-to-point uses both addresses (RFC 3021), hosts = **2**; **/32** is a single host route.
* **Private ranges (RFC1918):** `10.0.0.0/8`, `172.16.0.0/12`, `192.168.0.0/16`.

**Binary intuition:**
`IP AND Mask = Network`, `IP OR (NOT Mask) = Broadcast`.

#### Worked example A — Split a /24 into 4 equal subnets

* Start: `192.168.1.0/24`. Need 4 subnets ⇒ borrow **2 bits** ⇒ **/26**.
* Each /26 has `2^(32−26)=64` addresses → **62 usable** (-2).
* The four /26 subnets:

  1. **192.168.1.0/26** — usable `192.168.1.1–192.168.1.62`, broadcast `192.168.1.63`
  2. **192.168.1.64/26** — usable `.65–.126`, broadcast `.127`
  3. **192.168.1.128/26** — usable `.129–.190`, broadcast `.191`
  4. **192.168.1.192/26** — usable `.193–.254`, broadcast `.255`

#### Worked example B — Find network/broadcast for an arbitrary host

> Given **10.13.37.77/20**

* `/20` mask = `255.255.240.0` (block size 16 in the 3rd octet).
* 3rd octet is **37** → the /20 block starts at **32** (since floor(37/16)=2 → 2×16=32).
* **Network:** `10.13.32.0/20`
* **Broadcast:** `10.13.47.255` (next block 48 → 47.255 is last)
* **Usable range:** `10.13.32.1 – 10.13.47.254`
* **Hosts/subnet:** `2^(32−20) − 2 = 4096 − 2 = 4094`.

#### Worked example C — Need ≥6 subnets from a /24

* 6 requires 3 borrowed bits (2³=8) → **/27**.
* Hosts/subnet = `2^(32−27) − 2 = 32 − 2 = 30`.
* Networks: `.0/27, .32/27, .64/27, .96/27, .128/27, .160/27, .192/27, .224/27`.

**VLSM tip:** You can mix prefix lengths (e.g., one `/26` + two `/27` + one `/28`) to fit different host counts efficiently.

### 3.2 IPv6 subnetting in 60 seconds

* **No broadcast.** Use **multicast** and **Neighbor Discovery** (replaces ARP).
* Addresses are `prefix/interface-id`. Typical LANs are **/64**; SLAAC needs /64.
* **CIDR works the same**: `2001:db8:abcd:1200::/56` can be split into **256 /64s** (8 bits free).

  * They run from `2001:db8:abcd:1200::/64` to `2001:db8:abcd:12ff::/64`.
* Common practice:

  * ISP gives **/56** or **/48** to a site.
  * You assign each VLAN/segment a **/64**.
* **Specials:** Link-local `fe80::/10`; Unique Local (private) `fc00::/7` (commonly `fdxx::/8`); loopback `::1`.

---

## 4) TCP Three-Way Handshake (Connect) & Teardown

### Connect (3-Way Handshake)

```
Client                                Server
------                                ------
SYN (seq = x, options: MSS, WS, SACK, TS, …)  --->
                                   <---  SYN+ACK (seq = y, ack = x+1, options)
ACK (ack = y+1) ------------------------>
[Connection established]
```

* **Purpose:** agree on initial sequence numbers, negotiate options, create state.
* **Common options negotiated:**

  * **MSS** (Max Segment Size)
  * **Window Scaling** (bigger receive windows)
  * **SACK Permitted** (selective retransmit)
  * **Timestamps** (RTT measurement, PAWS)
* **Simultaneous open** is possible (both send SYN).
* **SYN backlog & SYN cookies:** protect against SYN floods.

### Teardown (orderly close)

```
A: FIN --->       (A done sending)
        <--- ACK
        <--- FIN  (B done sending)
ACK --->
A enters TIME_WAIT (typically 2*MSL) to absorb stragglers.
```

* **RST**: aborts immediately (no orderly close).

---

## 5) Flags & “types”

### 5.1 TCP flags (in the header)

| Flag    | Meaning                      | Typical use                                             |
| ------- | ---------------------------- | ------------------------------------------------------- |
| **SYN** | Synchronize sequence numbers | Connection start (SYN, SYN-ACK).                        |
| **ACK** | Acknowledgment valid         | Every segment after SYN usually has ACK set.            |
| **FIN** | Sender finished sending      | Orderly close (FIN/ACK).                                |
| **RST** | Reset                        | Abort connection (errors, closed ports).                |
| **PSH** | Push                         | Hint to deliver to app promptly (often seen with data). |
| **URG** | Urgent pointer valid         | Legacy; rarely used today.                              |
| **ECE** | ECN Echo                     | Congestion signaled by network (with ECN).              |
| **CWR** | Congestion Window Reduced    | Sender responded to ECN mark.                           |
| **NS**  | ECN nonce (obsolete)         | Rarely used/reserved bit historically.                  |

**Common combinations you’ll see:** `SYN`, `SYN+ACK`, `ACK`, `PSH+ACK`, `FIN+ACK`, `RST`.

### 5.2 IPv4 header flags (fragmentation)

* 3 bits: **Reserved(0)**, **DF** (Don’t Fragment), **MF** (More Fragments).

  * **DF=1**: router must not fragment; if packet too big, it drops and sends ICMP “Frag Needed” → **Path MTU Discovery**.
  * **MF=1**: more fragments follow; last fragment has MF=0.

### 5.3 ECN (Explicit Congestion Notification)

* IP header uses **ECT(0)**/**ECT(1)**/**CE** codepoints.
* If a router marks **CE**, receiver sets **ECE** in TCP ACKs; sender responds with **CWR** and reduces its congestion window. (QUIC does similar at its layer.)

---

## Quick cheat-sheet

* **Transport choices:** TCP (reliable), UDP (fast/lean), QUIC (modern over UDP), SCTP (multi-stream), DCCP (niche).
* **IPv6 vs IPv4:** IPv6 wins on scale, simplicity, and end-to-end; run **dual-stack** today.
* **Subnet math (IPv4):**

  * Hosts = `2^(32−prefix) − 2` (except /31 & /32).
  * Borrow **s** bits → `2^s` subnets.
  * Block size in an octet = `256 − mask_octet` (helps list ranges).
* **Handshake:** SYN → SYN/ACK → ACK; teardown uses FIN/ACK (or RST).
* **Flags:** TCP (`SYN, ACK, FIN, RST, PSH, URG, ECE, CWR`), IPv4 (`DF, MF`), ECN (ECT/CE + TCP ECE/CWR).

---

### Extras (handy defaults & tips)

* **Common IPv4 masks:**

  * `/30` (P2P): 4 addrs, 2 usable; `/31` (modern P2P): 2 addrs, **2 usable**; `/29`: 8 addrs, 6 usable; `/28`: 16/14; `/27`: 32/30; `/26`: 64/62; `/24`: 256/254.
* **Common IPv6 practice:** per LAN = **/64**. Site = **/56** or **/48**. No broadcast.
* **Troubleshooting mnemonics:**

  * If Path-MTU issues: expect black-holed large packets when **DF** is set and ICMP is filtered.
  * If “connection refused”: server replied with **RST** to a SYN on a closed port.
  * If “SYN sent, no reply”: firewall/drop, or SYN flood/backlog, or asym routing.


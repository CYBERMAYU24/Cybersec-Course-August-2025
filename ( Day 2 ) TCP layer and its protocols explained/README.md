
---

## 1) “TCP layer” protocols (Transport layer of TCP/IP)

| Protocol                                        | Orientation           | Reliability/Ordering                                                             | Congestion Ctrl              | Typical Uses                                | Notes                                                                                                                                       |
| ----------------------------------------------- | --------------------- | -------------------------------------------------------------------------------- | ---------------------------- | ------------------------------------------- | ------------------------------------------------------------------------------------------------------------------------------------------- |
| **TCP** (Transmission Control Protocol)         | Stream                | Reliable, ordered, byte-stream; retransmits lost data; flow control (rwnd); SACK | Yes (e.g., Reno, CUBIC, BBR) | Web (HTTP/1.1/2), SMTP, SSH, SQL            | Connection-oriented (3WH); supports **MSS**, **Window Scaling**, **Timestamps**, **SACK** via options.                                      |
| **UDP** (User Datagram Protocol)                | Message               | No reliability/order; best-effort                                                | No (apps must implement)     | DNS, VoIP, gaming, streaming, QUIC          | Very low overhead; preserves message boundaries. App-level ARQ/FEC if needed.                                                               |
| **SCTP** (Stream Control Transmission Protocol) | Message, multi-stream | Reliable & ordered **per stream**                                                | Yes                          | Telecom signaling, some WebRTC data         | Multi-homing (path failover), 4-way cookie handshake, less common on the public Internet.                                                   |
| **DCCP** (Datagram Congestion Control Protocol) | Message               | Unreliable; **with** congestion control                                          | Yes                          | Media where loss is OK but fairness matters | Experimental/rare.                                                                                                                          |
| **QUIC** (over UDP)                             | Stream, multiplexed   | Reliable streams (user-space)                                                    | Yes                          | **HTTP/3**, modern apps                     | Lives on UDP; built-in **TLS 1.3**, fast/0-RTT resume, better mobility. Not a kernel “TCP,” but effectively a modern transport used widely. |

**When to pick what?**

* **TCP**: when you need reliability + simplicity for developers.
* **UDP/QUIC**: when latency matters and you can tolerate/handle loss (streaming, real-time) or you want HTTP/3 features.
* **SCTP**: many streams without head-of-line blocking, multi-homing (niche).
* **DCCP**: academia/special cases.

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


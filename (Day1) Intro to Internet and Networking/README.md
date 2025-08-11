# History of the Internet

---

# 1. Early networks & ARPANET

* **ARPANET** (Advanced Research Projects Agency Network) was the first packet-switched network and the direct ancestor of the Internet.
* **Started**: late 1960s; first four-node network (UCLA, SRI, UCSB, University of Utah) connected in **1969**.
* **Key ideas**: packet switching, host-to-host communication, early protocols such as IMP (Interface Message Processor) and NCP (Network Control Protocol).
* ARPANET showed the practicality of interconnecting heterogeneous machines and introduced concepts that later evolved into TCP/IP.

# 2. NSFNET and commercialization of the Internet

* **NSFNET** (National Science Foundation Network) was created in the mid-1980s to provide high-speed backbone services for research and education in the U.S.
* NSFNET replaced the ARPANET backbone and provided broader connectivity between universities and research centers.
* By the early 1990s, commercial networks and ISPs took over backbone roles; **NSFNET was decommissioned in 1995**, which contributed to the Internet’s commercial expansion.

# 3. The World Wide Web (WWW)

* **Invented** by Sir Tim Berners-Lee at CERN in **1989–1990** as a system to link and share documents via **HTTP** and **HTML**.
* **First web server & browser**: early 1990s at CERN. The Web layered on top of TCP/IP and made the Internet far more accessible to non-technical users via hyperlinked documents.
* The Web is an application-level service (HTTP, HTML, URLs) that enabled modern websites and web services.

# 4. Early search engines — Archie (and friends)

* **Archie** (1990) — earliest tool to index FTP file listings so users could search for filenames across FTP servers (not full-text indexing). Often called the first “search engine.”
* Related early systems:

  * **Gopher** (menu-based distributed document system) and search tools like **Veronica** and **Jughead** for Gopher menus.
  * Early web search/crawlers (mid-1990s) evolved into and beyond Archie’s concept when the Web grew.

# 5. Mail services (brief history + how mail works)

* **Early email** existed on ARPANET in the 1970s; it became one of the most popular ARPANET applications.
* **Hotmail**: launched in **1996** (Sabeer Bhatia & Jack Smith) — one of the major early **webmail** services, popularizing web-based email accessible from any browser.
* **How Internet email works (high level)**:

  1. **Submission** from client → MUA (Mail User Agent, e.g., Outlook) sends to MTA (Mail Transfer Agent) using SMTP.
  2. **Transfer** between MTAs uses **SMTP** (Simple Mail Transfer Protocol).
  3. **Storage**: recipient MTA stores mail in mailbox.
  4. **Retrieval**: recipient MUA fetches mail using **POP3** or **IMAP** (IMAP supports server-side folders & sync).
* Protocols:

  * `SMTP` for sending/relaying mail.
  * `POP3` (download-and-delete model) and `IMAP` (server-side synchronization) for retrieval.
  * Webmail interfaces run on top of HTTP.

# 6. Naming & addressing on the Internet

**Key components**

* **IP addresses**: numeric addresses assigned to network interfaces.

  * `IPv4` — 32-bit addresses (e.g., `192.0.2.1`).
  * `IPv6` — 128-bit addresses to solve IPv4 exhaustion (e.g., `2001:db8::1`).
* **DNS (Domain Name System)**:

  * Invented by Paul Mockapetris (1983) to translate human-friendly names (e.g., `example.com`) into IP addresses.
  * Hierarchical: root → TLDs (.com, .org, country-code) → authoritative name servers.
  * Supports **A** records (IPv4), **AAAA** (IPv6), **MX** (mail exchange), **CNAME**, **NS**, etc.
* **DHCP** (Dynamic Host Configuration Protocol):

  * Automatically assigns IP addresses and options (gateway, DNS servers) to hosts on a network.
* **NAT** (Network Address Translation):

  * Lets multiple private hosts share a single public IPv4 address; widely used in home routers.
* **Reverse DNS** mappings and PTR records let IPs map back to names.
* **Addressing examples**: IPv4 private ranges (`10.0.0.0/8`, `192.168.0.0/16`) and public routable addresses.

# 7. Who came first — TCP or OSI?

* **TCP/IP** development (protocols and implementation) began in the **1970s** as part of DARPA research; the **TCP/IP protocols were implemented and running on networks by the late 1970s / early 1980s** and became the standard for ARPANET/Internet.
* The **OSI model** (Open Systems Interconnection) was developed later by ISO (late 1970s, standardized in the early-mid 1980s) as a *conceptual reference model* for network communications.
* **Short answer**: **TCP/IP (as protocols) came before the OSI reference model became formalized.** OSI was a conceptual framework developed around the time TCP/IP was maturing but was not the original implementation that created the Internet.

# 8. OSI model — 7 layers (explanation & examples)

> The OSI (Open Systems Interconnection) model is a conceptual 7-layer framework used to understand and design interoperable network systems.

1. **Layer 7 — Application**

   * What it does: Exposes network services to applications and users.
   * Examples/protocols: `HTTP`, `FTP`, `SMTP`, `DNS`, `Telnet`.
2. **Layer 6 — Presentation**

   * What it does: Data representation, encryption/decryption, compression.
   * Examples: `SSL/TLS` (often thought of between presentation and session in practice), character encoding (UTF-8).
3. **Layer 5 — Session**

   * What it does: Manages sessions/ dialogues, connection setup/teardown, checkpointing.
   * Examples: Session management in RPC systems, some aspects of NetBIOS sessions.
4. **Layer 4 — Transport**

   * What it does: End-to-end communication, reliability, flow & congestion control.
   * Examples/protocols: `TCP` (reliable, connection-oriented), `UDP` (unreliable, connectionless).
5. **Layer 3 — Network**

   * What it does: Logical addressing and routing between networks.
   * Examples/protocols: `IP` (IPv4/IPv6), ICMP, routing protocols (OSPF, BGP).
6. **Layer 2 — Data Link**

   * What it does: Framing, local physical addressing (MAC), error detection, link control.
   * Examples: `Ethernet` (IEEE 802.3), Wi-Fi (IEEE 802.11), PPP.
7. **Layer 1 — Physical**

   * What it does: Raw bit transmission over physical medium.
   * Examples: Cables (Ethernet wiring), fiber optics, radio waves, voltages and line encoding.

**Note**: OSI is mainly pedagogical — in real-world networking, layers can blur and protocols may span multiple OSI layers.

# 9. TCP/IP (Internet) model — layers & mapping to OSI

The Internet (TCP/IP) model is simpler and more implementation-centered. A common 4-layer variant:

1. **Link (Network Interface) layer**

   * Maps to OSI L1 (Physical) + L2 (Data Link).
   * Examples: Ethernet, Wi-Fi, PPP.
2. **Internet layer**

   * Maps mainly to OSI L3 (Network).
   * Examples: `IP` (IPv4/IPv6), ICMP, routing.
3. **Transport layer**

   * Maps to OSI L4 (Transport).
   * Examples: `TCP`, `UDP`.
4. **Application layer**

   * Combines OSI L5–L7 (Session, Presentation, Application).
   * Examples: `HTTP`, `SMTP`, `DNS`, `FTP`.

**Simple mapping diagram (conceptual)**:

```
OSI:    7 App   6 Pres   5 Sess   4 Trans   3 Net   2 DataLink   1 Physical
TCP/IP:        Application        |   Transport   |   Internet  |  Link
```

# 10. TCP vs UDP — detailed differences (table + use cases)

### Quick summary

* **TCP** = *Transmission Control Protocol* — connection-oriented, reliable, ordered, congestion-controlled.
* **UDP** = *User Datagram Protocol* — connectionless, unreliable (best-effort), lower overhead, useful for simple queries and real-time apps.

### Feature-by-feature comparison

| Feature                       |                                              TCP | UDP                                                                             |
| ----------------------------- | -----------------------------------------------: | ------------------------------------------------------------------------------- |
| Connection                    |                  Connection-oriented (handshake) | Connectionless                                                                  |
| Reliability                   |        Reliable delivery (ACKs, retransmissions) | Unreliable (no retransmit by protocol)                                          |
| Ordering                      |                      Preserves byte-stream order | No ordering guarantee                                                           |
| Flow control                  |                            Yes (sliding windows) | No                                                                              |
| Congestion control            |           Yes (slow start, congestion avoidance) | No                                                                              |
| Header size                   |               Minimum 20 bytes (without options) | 8 bytes                                                                         |
| Overhead                      |                                           Higher | Lower                                                                           |
| Use cases                     | Web (HTTP/HTTPS), SMTP, FTP, SSH, file transfers | DNS queries, streaming (audio/video), VoIP, online games, simple query/response |
| Ports                         |                      Uses port numbers, like UDP | Uses port numbers, like TCP                                                     |
| Typical reliability mechanism |               Retransmit, ACKs, sequence numbers | Application must handle reliability if needed                                   |

### TCP details: three-way handshake (brief)

1. `Client -> Server: SYN` (synchronize: client wants to open connection)
2. `Server -> Client: SYN-ACK` (server acknowledges and synchronizes)
3. `Client -> Server: ACK` (client confirms)
   After this, a reliable byte-stream begins. Closing uses FIN/ACK sequences.

### UDP details: behavior

* UDP sends independent datagrams. If a datagram is lost, duplicated, or arrives out of order, the protocol itself does not correct it — the application must handle those issues if needed.
* Ideal when low-latency or lower overhead matters more than perfect reliability (e.g., real-time voice/video).

### When to choose which?

* Choose **TCP** when data must be accurate and complete (web pages, file transfers, email).
* Choose **UDP** when speed/low-latency or multicast/broadcast support matters, and either occasional loss is acceptable or the application implements its own reliability (DNS, live audio/video, gaming).

---

## Extra: Common protocol-port examples

* `HTTP` → port `80` (TCP)
* `HTTPS` → port `443` (TCP)
* `DNS` → port `53` (UDP for queries; TCP for zone transfers)
* `SMTP` → port `25` (TCP)
* `POP3` → port `110` (TCP)
* `IMAP` → port `143` (TCP)
* `SSH` → port `22` (TCP)

---

## Quick bibliography / further reading suggestions

* RFCs for protocols (e.g., RFC 791 for IPv4, RFC 793 for TCP, RFC 768 for UDP).
* Tim Berners-Lee’s original Web proposal and CERN documentation.
* "Computer Networks" by Andrew S. Tanenbaum — good textbook for OSI vs TCP/IP concepts.
* DNS and DHCP RFCs (e.g., RFC 1034/1035 for DNS, RFC 2131 for DHCP).

---


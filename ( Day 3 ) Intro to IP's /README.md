
# Networking Concepts

## 1. `testphp.vuln` Website – Purpose

`testphp.vulnweb.com` is a **deliberately vulnerable web application** maintained by *Acunetix* for **learning and testing web security tools**.
It is commonly used for:

* **Practicing penetration testing** (SQL injection, XSS, LFI, etc.).
* **Training cybersecurity students** on real vulnerabilities without harming real websites.
* **Testing vulnerability scanners** to ensure detection tools work correctly.

⚠ **Important:**
This site is **intentionally insecure** and should **only** be used for ethical security testing and learning, never for malicious purposes.

---

## 2. IP Address – Definition, Types & Categories

### **What is an IP Address?**

An **IP (Internet Protocol) Address** is a **unique numerical identifier** assigned to each device connected to a network that uses the Internet Protocol.

### **Types of IP Addresses**

* **IPv4 (Internet Protocol version 4)**

  * 32-bit address
  * Written in dotted decimal format (e.g., `192.168.1.1`)
  * Around 4.3 billion unique addresses
* **IPv6 (Internet Protocol version 6)**

  * 128-bit address
  * Written in hexadecimal format with colons (e.g., `2001:0db8:85a3::8a2e:0370:7334`)
  * Provides virtually unlimited addresses

### **Categories by Function**

1. **Public IP**

   * Accessible over the internet
   * Assigned by **ISP**
   * Unique globally
2. **Private IP**

   * Used within local/private networks
   * Not routable on the internet
   * Defined ranges (RFC 1918 for IPv4):

     * `10.0.0.0 – 10.255.255.255`
     * `172.16.0.0 – 172.31.255.255`
     * `192.168.0.0 – 192.168.255.255`
3. **Static IP**

   * Fixed address that doesn’t change
4. **Dynamic IP**

   * Assigned temporarily by DHCP server

---

## 3. IPv4 Classes

IPv4 addresses are divided into **five classes** based on their **first octet**.

| Class | First Octet Range | Default Subnet Mask | No. of Networks | Hosts per Network | Usage               |
| ----- | ----------------- | ------------------- | --------------- | ----------------- | ------------------- |
| **A** | 1 – 126           | 255.0.0.0 (/8)      | 126             | 16,777,214        | Very large networks |
| **B** | 128 – 191         | 255.255.0.0 (/16)   | 16,384          | 65,534            | Medium networks     |
| **C** | 192 – 223         | 255.255.255.0 (/24) | 2,097,152       | 254               | Small networks      |
| **D** | 224 – 239         | N/A                 | N/A             | N/A               | Multicasting        |
| **E** | 240 – 255         | N/A                 | N/A             | N/A               | Experimental        |

> **Note:** 127.x.x.x is reserved for loopback testing (localhost).

---

## 4. IPv4 Header Structure

IPv4 packets have a header that contains **control information** for routing and delivery.

**IPv4 Header Fields:**

| Field                      | Size (bits) | Description                              |
| -------------------------- | ----------- | ---------------------------------------- |
| **Version**                | 4           | IP version (4 for IPv4)                  |
| **IHL**                    | 4           | Internet Header Length                   |
| **Type of Service (ToS)**  | 8           | QoS parameters                           |
| **Total Length**           | 16          | Entire packet length                     |
| **Identification**         | 16          | Unique ID for fragmentation              |
| **Flags**                  | 3           | Fragmentation control                    |
| **Fragment Offset**        | 13          | Position of fragment                     |
| **Time To Live (TTL)**     | 8           | Packet lifespan                          |
| **Protocol**               | 8           | Transport layer protocol (TCP=6, UDP=17) |
| **Header Checksum**        | 16          | Error-checking value                     |
| **Source IP Address**      | 32          | Sender’s IP                              |
| **Destination IP Address** | 32          | Receiver’s IP                            |
| **Options + Padding**      | Variable    | Extra settings                           |

---

## 5. Header Checksum (IPv4)

* **Purpose:** Detect errors in the header during transmission.
* **How it works:**

  1. Sender calculates checksum by adding all 16-bit words in header and taking 1’s complement.
  2. Receiver calculates checksum again and compares.
  3. If values differ → packet discarded.
* **Note:** Only checks **header**, not the data payload.

---

## 6. IPv6 Header Structure

IPv6 simplifies the header compared to IPv4.

**IPv6 Header Fields:**

| Field                   | Size (bits) | Description                                 |
| ----------------------- | ----------- | ------------------------------------------- |
| **Version**             | 4           | IP version (6 for IPv6)                     |
| **Traffic Class**       | 8           | QoS, priority                               |
| **Flow Label**          | 20          | Identifies packet flow for special handling |
| **Payload Length**      | 16          | Size of data after header                   |
| **Next Header**         | 8           | Indicates next protocol (TCP, UDP, etc.)    |
| **Hop Limit**           | 8           | Replaces TTL                                |
| **Source Address**      | 128         | Sender’s IPv6                               |
| **Destination Address** | 128         | Receiver’s IPv6                             |

**Key IPv6 Differences:**

* No header checksum (error checking handled by lower layers like TCP/UDP)
* Fixed header size: 40 bytes
* More efficient routing

---

## 7. Public vs Private IP

| Aspect              | Public IP                                  | Private IP                           |
| ------------------- | ------------------------------------------ | ------------------------------------ |
| **Access**          | Accessible from anywhere over the internet | Accessible only within local network |
| **Assigned By**     | ISP                                        | Router/DHCP server                   |
| **Uniqueness**      | Globally unique                            | Can be reused in different networks  |
| **Example**         | `8.8.8.8` (Google DNS)                     | `192.168.1.1`                        |
| **Security**        | More exposed to attacks                    | Safer from direct external threats   |
| **NAT Requirement** | Not needed                                 | Needs NAT to access the internet     |

---

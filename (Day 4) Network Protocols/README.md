
---

## **1. DHCP (Dynamic Host Configuration Protocol)**

**Definition:**
DHCP is a network management protocol used to automatically assign IP addresses, subnet masks, default gateways, and DNS server addresses to devices on a network.

**Purpose:**

* Avoids manual IP configuration for each device.
* Ensures IP address uniqueness (prevents conflicts).
* Supports both **IPv4** and **IPv6**.

**How it Works:**

* DHCP operates in a **client-server model**.
* A DHCP server holds a pool of IP addresses and configuration parameters.
* When a device connects to the network, it requests an IP from the server.

---

## **2. DORA Process in DHCP**

DORA is the **4-step handshake** used in DHCP for IPv4 address allocation:

1. **D – Discover**

   * **Client → Broadcast**
   * The client sends a DHCP Discover message to locate available DHCP servers.
   * Uses **Broadcast** (`255.255.255.255`).

2. **O – Offer**

   * **Server → Broadcast**
   * DHCP server responds with an IP offer, subnet mask, lease time, and other configuration details.

3. **R – Request**

   * **Client → Broadcast**
   * The client requests to accept the offered IP (may reject others).

4. **A – Acknowledge**

   * **Server → Unicast/Broadcast**
   * The DHCP server confirms allocation and finalizes the lease.

**Diagram:**

```
Client                      Server
   |---DHCP Discover------->|
   |<--DHCP Offer-----------|
   |---DHCP Request-------->|
   |<--DHCP Acknowledge-----|
```

---

## **3. Capturing DORA Process in Wireshark**

**Steps:**

1. **Open Wireshark** and select the network interface (e.g., Ethernet or Wi-Fi).
2. In the **capture filter**, type:

   ```
   port 67 or port 68
   ```

   This filters DHCP packets (UDP ports 67 for server, 68 for client).
3. **Start Capture** before the client connects to the network.
4. **Release and Renew IP**:

   * Windows:

     ```
     ipconfig /release
     ipconfig /renew
     ```
   * Linux/Mac:

     ```
     sudo dhclient -r
     sudo dhclient
     ```
5. Stop the capture and look for:

   * **Discover** (Broadcast)
   * **Offer** (Broadcast)
   * **Request** (Broadcast)
   * **Acknowledge** (Broadcast/Unicast)

---

## **4. Unicast vs Broadcast**

| **Type**      | **Definition**                                               | **Destination**     | **Example Use**  |
| ------------- | ------------------------------------------------------------ | ------------------- | ---------------- |
| **Unicast**   | Data sent from one sender to one receiver.                   | Specific IP address | Sending an email |
| **Broadcast** | Data sent from one sender to **all devices** on the network. | `255.255.255.255`   | DHCP Discover    |

**Key Points:**

* **Unicast** is efficient for one-to-one communication.
* **Broadcast** is useful for discovery protocols like ARP and DHCP but can cause unnecessary load if overused.

---

## **5. ARP (Address Resolution Protocol)**

**Definition:**
ARP is used to map an **IP address** (logical address) to a **MAC address** (physical address) on a local network.

**Why It’s Used:**

* Devices communicate using MAC addresses at the Data Link Layer.
* IP addresses need to be translated to MAC addresses for actual delivery in a LAN.

**How It Works:**

1. If a device wants to send data to another IP in the same network, it checks its **ARP cache**.
2. If no mapping exists:

   * Sends a **Broadcast ARP Request**: *"Who has IP 192.168.1.5?"*
3. The device with that IP replies with its MAC address (**Unicast ARP Reply**).
4. The sender stores this mapping in its **ARP cache** for faster future communication.

---

## **6. ARP Cache Poisoning**

**Definition:**
A type of cyber attack where a malicious actor sends **fake ARP replies** to associate their MAC address with the IP address of another device (e.g., the gateway).

**How It Works:**

1. Attacker sends forged ARP replies to the victim: *"Gateway IP (192.168.1.1) is at Attacker's MAC"*
2. Victim updates ARP cache with wrong mapping.
3. Traffic intended for the gateway is sent to the attacker instead.
4. Attacker can:

   * Intercept traffic (Man-in-the-Middle).
   * Modify or block data.

**Prevention:**

* Use **Static ARP entries** for critical devices.
* Enable **Dynamic ARP Inspection (DAI)** on switches.
* Use encryption (HTTPS, VPN) to prevent reading sensitive data even if intercepted.

---

## **Summary Table**

| Concept           | Purpose                             | Communication Type  |
| ----------------- | ----------------------------------- | ------------------- |
| **DHCP**          | Automatic IP assignment             | Broadcast & Unicast |
| **DORA**          | 4-step IP allocation                | Mostly Broadcast    |
| **ARP**           | IP to MAC resolution                | Broadcast & Unicast |
| **ARP Poisoning** | Hijack traffic via fake ARP entries | Broadcast & Unicast |

---

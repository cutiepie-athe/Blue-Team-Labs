# pfSense Configuration

This file contains the basic pfSense configuration used for the cybersecurity lab setup.  

---

## 🔹 Interfaces
- **WAN**: DHCP (connected to VirtualBox NAT network)  
  - pfSense automatically receives an IP from VirtualBox (e.g., `10.0.2.x`).  
- **LAN**: Static IP → `192.168.1.1/24`  
  - Acts as the default gateway for all lab machines.  
  - Example IPs:
    - Active Directory Server: `192.168.1.10`
    - Windows Workstation: `192.168.1.20`

---

## 🔹 Firewall Rules
- **Allow LAN → Any**  
  - Permits all LAN clients to access external networks.  
- **Allow ICMP (Ping) from LAN**  
  - Useful for connectivity testing.  
- **Block all inbound WAN traffic (default)**  
  - Protects LAN from outside attacks.  

---

## 🔹 NAT (Network Address Translation)
- **Automatic Outbound NAT** → Enabled  
  - Translates LAN private IPs (`192.168.1.x`) into the WAN IP.  
  - Ensures LAN clients can reach the internet.  

---

## 🔹 Services
- **DHCP Server (LAN)**  
  - Enabled with the following range:  
    - `192.168.1.100 – 192.168.1.200`  
  - Clients automatically receive:
    - IP address
    - Default gateway → `192.168.1.1`
    - DNS resolver → `192.168.1.1`
- **DNS Resolver** → Enabled  
  - Resolves DNS queries for LAN clients.  
  - Can later integrate with AD DNS if needed.  

---

✅ With this configuration, pfSense provides internet access, firewall protection, and DHCP/DNS services to the lab environment.  

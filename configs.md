# pfSense Configuration

## Interfaces
- **WAN**: DHCP (connected to NAT)
- **LAN**: Static IP → 192.168.1.1/24

## Firewall Rules
- Allow LAN → Any
- Allow ICMP (Ping) from LAN
- Block all inbound WAN traffic (default)

## NAT
- Enabled Automatic Outbound NAT

## Services
- DHCP Server enabled on LAN:
  - Range: 192.168.1.100 – 192.168.1.200
- DNS Resolver enabled

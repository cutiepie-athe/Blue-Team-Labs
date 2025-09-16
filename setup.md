# pfSense Setup Guide

## Objective
Deploy pfSense as a firewall and router in the cybersecurity lab.

## Steps
1. Download pfSense ISO from the official site.
2. Create a VM in VirtualBox with:
   - 2 GB RAM
   - 20 GB disk
   - 2 Network Adapters:
     - Adapter 1 → NAT (for WAN/Internet)
     - Adapter 2 → Internal Network (for LAN)
3. Boot with the pfSense ISO and install with default options.
4. Assign:
   - WAN → em0 (NAT)
   - LAN → em1 (Internal Network)
5. Access pfSense WebConfigurator at:  
   `https://192.168.1.1`  
   Default login: `admin / pfsense`

# Blue-Team-Labs

# Simple-pfSense-set-up

## Objective
The primary focus of this project was to gain basic familiarity of another firewall software in a virtualized environment. After pfSense set-up was complete, very simple firewall rules were created on my own in a Windows virtual machine and experimented with to primarily learn rule components, rule ordering and processing.

Sources used for this set-up: [MyDFIR on YouTube](https://www.youtube.com/watch?v=IymmPht-dAQ), [Arcy Caparros](https://arcy24.medium.com/setting-home-lab-via-pfsense-part-1-5dffd73d6871), [Joshua Greene](https://drive.google.com/file/d/1bYiYn-L6si4Q27qhIIxpfigM0Ax9PofH/view), [ToTatCa on YouTube](https://www.youtube.com/watch?v=phwrzv8KlUE&t=624s)

### Skills Learned
- Installing and setting up pfSense
- Configuring NICs on VirtualBox (VB) and assigning interfaces within pfSense
- Creating simple firewall rules and seeing its results using _ping_ command

### Tools Used
- pfSense as the open source firewall
- VirtualBox as the hypervisor 
- CMD commands (ping & ipconfig)

### Takeaways
This project made me realize that setting up the environment can feel more complicated than configuring the software. Multiple sources are listed because the LAN IP in the pfSense shell did not match the default gateway in the Windows machine. All sources contributed in getting familiar with setting up pfSense. Rules creation at the end was a trial and error process on my own to see their impact on pings from the Windows virtual machine to the default gateway which is also the pfSense LAN IP address. Biggest takeaway is managing firewalls in an enterprise setting sounds very complicated and intricate. 

### Network Diagram
<p align="center">
<img src="https://i.imgur.com/Y6Mm7av.png" height="50%" width="50%" alt="pfSense Steps"/>
<br />
  
### Steps

Download pfSense iso on host machine at https://www.pfsense.org/download/ > purchase Netgate Installer with the image you want > verify SHA256 checksum > extract file > Create new Virtual Machine (VM) in VB.
<p align="center">
Purchase Netgate installer: <br/>
<img src="https://i.imgur.com/hGZkg7E.png" height="30%" width="30%" alt="pfSense Steps"/>
<br />
Download selected iso image: <br/>
<img src="https://i.imgur.com/o9fltTc.png" height="40%" width="40%" alt="pfSense Steps"/>
<br />
Verify SHA256 checksum - checksum matches: <br/>
<img src="https://i.imgur.com/JjnG0So.png" height="70%" width="70%" alt="pfSense Steps"/>
<br />
Extract file: <br/>
<img src="https://i.imgur.com/7jKMCYC.png" height="60%" width="60%" alt="pfSense Steps"/>
<br />
Create new VM in VB: <br/>
<img src="https://i.imgur.com/JKCPtsB.png" height="40%" width="40%" alt="pfSense Steps"/>
<br />
<br />
  
Need to add a network adapter for firewall machine in VB, Tools > Network > Properties > Create new network named pfsense in NAT Networks > assign 192.168.50.0 and enable DHCP > Apply. 
<p align="center">
VB Tools > Network: <br/>
<img src="https://i.imgur.com/7ite07E.png" height="30%" width="30%" alt="pfSense Steps"/>
<br />
Create a new NAT network: <br/>
<img src="https://i.imgur.com/VcrYL2b.png" height="20%" width="20%" alt="pfSense Steps"/>
<br />
Assign an IP address and enable DHCP: <br/>
<img src="https://i.imgur.com/cYhM4mW.png" height="40%" width="40%" alt="pfSense Steps"/>
<br />
<br />
  
Firewall typically uses two interfaces, one for WAN and one for LAN. A type two hypervisor like VB only has one physical NIC card so this set-up will have a WAN private IP instead of a normal public IP. <br />
In the pfSense VM settings > Network > Adapter 1 = Bridged adapter > Adapter 2 = NAT Network created earlier > OK. <br />
Bridged adapter means the pfSense VM will obtain the same IP on the same network as the host machine
<p align="center">
pfSense VM adapter 1 & 2 configurations: <br/>
<img src="https://i.imgur.com/Kaum6mh.png" height="30%" width="30%" alt="pfSense Steps"/>
<br />
<br />

Install pfSense by starting the pfSense VM in VB > Accept terms & conditions > Install pfSense. 
<p align="center">
Install pfSense: <br/>
<img src="https://i.imgur.com/CXzW0CJ.png" height="40%" width="40%" alt="pfSense Steps"/>
<br />
<br />

Select and assign the WAN & LAN interfaces in pfSense. The WAN interface is the bridged adapter with the correspoding MAC address. LAN interface is the pfSense NAT network with the corresponding MAC address.
<p align="center">
WAN interface = le0 = network adapter 1 = bridged adapter. Leave network operation mode default: <br/>
<img src="https://i.imgur.com/fClSo4f.png" height="40%" width="40%" alt="pfSense Steps"/>
<br />
LAN interface = le1 = network adapter 2 = NAT network. pfSense has its own DHCP server so it is assigning it's own IP > leave network operation mode default: <br/>
<img src="https://i.imgur.com/GTx8hc0.png" height="40%" width="40%" alt="pfSense Steps"/>
<br />
Select version to install: <br/>
<img src="https://i.imgur.com/U33Jaum.png" height="30%" width="30%" alt="pfSense Steps"/>
<br />
Installation complete: <br/>
<img src="https://i.imgur.com/dJYHYvl.png" height="30%" width="30%" alt="pfSense Steps"/>
<br />
<br />

After pfSense installation is complete, the iso image needs to be removed from pfSense VM storage settings in order for the system to boot from the virtual disk where it was installed. If the iso image is not removed, pfSense will continue rebooting into its terms and conditions notice. Start the pfSense VM again after.
<p align="center">
Power off machine: <br/>
<img src="https://i.imgur.com/1LomB4i.png" height="30%" width="30%" alt="pfSense Steps"/>
<br />
Remove iso image in pfSense VM settings > storage: <br/>
<img src="https://i.imgur.com/ZQIwuSo.png" height="50%" width="50%" alt="pfSense Steps"/>
<br />
Result after removing iso image: <br/>
<img src="https://i.imgur.com/3tgflBz.png" height="20%" width="20%" alt="pfSense Steps"/>
<br />
Start pfSense VM > No if VLANs should be set up now > input correct WAN & LAN interfaces > confirm choices: <br/>
<img src="https://i.imgur.com/8sEkf1L.png" height="40%" width="40%" alt="pfSense Steps"/>
<br />
<br />

In a real-life environment, the default WAN IP address will likely be our public IP address. Assigning the same NAT network to another VM should result in it being in the same network as the pfSense machine (in theory). <br />
Confirm the IP addresses in the Windows VM by using command prompt. We want to see the Windows VM default gateway IP address to match the pfSense LAN IP address. 
<p align="center">
Assign NAT Network created earlier to the pfSense Windows machine: <br/>
<img src="https://i.imgur.com/YLG3gWj.png" height="30%" width="30%" alt="pfSense Steps"/>
<br />
Check IPv4 address in the Windows machine. IPv4 address automatically obtained is fine: <br/>
<img src="https://i.imgur.com/D4rbeld.png" height="30%" width="30%" alt="pfSense Steps"/>
<br />
Open CMD and input command "ipconfig". Default gateway IP address=192.168.1.1: <br/>
<img src="https://i.imgur.com/yzFbG7o.png" height="50%" width="50%" alt="pfSense Steps"/>
<br />
LAN IP address in pfSense shell matches Windows VM default gateway IP: <br/>
<img src="https://i.imgur.com/HEOoNxL.png" height="50%" width="50%" alt="pfSense Steps"/>
<br />
<br />

Default gateway IP address matching the LAN IP address in pfSense machine shell means that the pfSense GUI can likely be accessed by searching the gateway address in the Windows machine browser search bar. A warning will prompt as HTTP is the default protocol. 
<p align="center">
Warning prompt > Advaced > Continue to 192.168.1.1: <br/>
<img src="https://i.imgur.com/ewWbhLB.png" height="40%" width="40%" alt="pfSense Steps"/>
<br />
Now able to access the pfSense web interface: <br/>
<img src="https://i.imgur.com/eP9FQ81.png" height="40%" width="40%" alt="pfSense Steps"/>
<br />
<br />

Default credentials are "admin" & "pfsense". After signing in, a set-up wizard will occur. For the purposes of this project, majority of the configurations will be left default except for RFC1918 Networks configuration. <br /> 
Block RFC1918 Private Networks is checked by default but since the WAN network in this project is in a private address space (192.168.x.x), it needs to be unchecked. 
<p align="center">
pfSense set-up wizard: <br/>
<img src="https://i.imgur.com/GsMBnW9.png" height="40%" width="40%" alt="pfSense Steps"/>
<br />
In step 4 of the set-up wizard - uncheck "Block RFC1918 Private Networks": <br/>
<img src="https://i.imgur.com/RLhinEw.png" height="50%" width="50%" alt="pfSense Steps"/>
<br />
In step 5 of the set-up wizard - leave default (should be the gateway address): <br/>
<img src="https://i.imgur.com/9BBhzrO.png" height="40%" width="40%" alt="pfSense Steps"/>
<br />
In step 6 of the set-up wizard - set a different admin password: <br/>
<img src="https://i.imgur.com/84dUkdh.png" height="40%" width="40%" alt="pfSense Steps"/>
<br />
Reload configuration & Finish: <br/>
<img src="https://i.imgur.com/bvRtTS7.png" height="40%" width="40%" alt="pfSense Steps"/>
<br />
<img src="https://i.imgur.com/JdBEFiW.png" height="40%" width="40%" alt="pfSense Steps"/>
<br />
<br />

In the pfSense webgui homepage, WAN & LAN interfaces should match the addresses in the pfSense shell. 
<p align="center">
WAN & LAN addresses match: <br/>
<img src="https://i.imgur.com/5E7w7cG.png" height="40%" width="40%" alt="pfSense Steps"/>
<br />
<br />

pfSense provides many tools/3rd-party packages that you can install and use, including starting your own VPN server(s). Toolbar at the top > System > Package Manager > Available Packages > Search
<p align="center">
For example: Snort package (IDS/IPS): <br/>
<img src="https://i.imgur.com/UkYyy0w.png" height="40%" width="40%" alt="pfSense Steps"/>
<br />
<br />
  
Firewall rules can be created to block/allow traffic in or out of your environment. Toolbar at the top > Firewall > Rules. 
<p align="center">
Navigate to firewall rules: <br/>
<img src="https://i.imgur.com/bS6yaYg.png" height="10%" width="10%" alt="pfSense Steps"/>
<br />
Firewall rules landing page: <br/>
<img src="https://i.imgur.com/qlSfpeY.png" height="60%" width="60%" alt="pfSense Steps"/>
<br />
<br />

After basic set-up of pfSense, my goal was to create a few simple rules so pings from the Windows VM to the default gateway (pfSense shell LAN IP address) succeeds or fails. <br />
Rule #1 function: block any traffic from any source to the destination IP address of 192.168.1.1. When the rule is enabled, we should see pings fail. When the rule is disabled, we should see pings succeed. <br />
Rule #2 function: similar rule as rule #1 but instead allow any traffic from any source to the destination IP address of 192.168.1.1. 
<p align="center">
Pings to 192.168.1.1 from Windows VM succeed initially: <br/>
<img src="https://i.imgur.com/57rW1Xc.png" height="40%" width="40%" alt="pfSense Steps"/>
<br />
Firewall > Rules > LAN > either direction green Add button > rule set-up > Save: <br/>
<img src="https://i.imgur.com/I780ueD.png" height="40%" width="40%" alt="pfSense Steps"/>
<br />
After changes have been applied: <br/>
<img src="https://i.imgur.com/5fIzDah.png" height="60%" width="60%" alt="pfSense Steps"/>
<br />
Pings to 192.168.1.1 from Windows VM now fail with pfSense Wins block enabled: <br/>
<img src="https://i.imgur.com/YggSc8F.png" height="40%" width="40%" alt="pfSense Steps"/>
<br />
Disable the rule to verify pings succeed again: <br/>
<img src="https://i.imgur.com/YUPStWb.png" height="70%" width="70%" alt="pfSense Steps"/>
<br />
Pings succeed with the rule disabled: <br/>
<img src="https://i.imgur.com/BY7pcni.png" height="40%" width="40%" alt="pfSense Steps"/>
<br />
Re-enable the rule and apply changes: <br/>
<img src="https://i.imgur.com/2vuXk4A.png" height="50%" width="50%" alt="pfSense Steps"/>
<br />
<br />

Create rule #2 but the Action is "Pass" instead of Block with all other details the same > Apply changes. Rules are ordered from top-to-bottom so rules at the top overrule those below them. This is an oversimplification but the images and steps below illustrate the top-to-bottom order. <br/>
Allow rule is _above_ the Block rule so pings should succeed. If Allow rule is _below_ the Block rule, then the Block rule takes precedent with the pings failing.  
<p align="center">
Rule Action - Pass: <br/>
<img src="https://i.imgur.com/Wh83zrg.png" height="50%" width="50%" alt="pfSense Steps"/>
<br />
Two different rules: <br/>
<img src="https://i.imgur.com/wjstI6y.png" height="60%" width="60%" alt="pfSense Steps"/>
<br />
Successful pings: <br/>
<img src="https://i.imgur.com/u3uACgJ.png" height="40%" width="40%" alt="pfSense Steps"/>
<br />
Moving the allow rule below the block rule: <br/>
<img src="https://i.imgur.com/9zXzmUs.png" height="60%" width="60%" alt="pfSense Steps"/>
<br />
Pings failing: <br/>
<img src="https://i.imgur.com/cHWkJ4y.png" height="40%" width="40%" alt="pfSense Steps"/>
<br />


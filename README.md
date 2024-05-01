# AD-Lab-with-Splunk

## Objective

In this project, we have set up one Active Directory Server, one Windows 10 PC, a Kali Linux that simulates an attacker, and a Splunk server running on Ubuntu. 

The project is based on <a href="https://www.youtube.com/@MyDFIR"> @MyDFIR </a> Active Directory Project (Home Lab).

### Skills Learned

- Basic introduction to Splunk
- Basic introduction to Active Directory
- Basic introduction to Kali Linux and Ubuntu
- Basic intruduction to crowbar, Windows Event Code
- Basic introduction to Atomic Red Team and MITRE ATT&CK framework

### Tools Used
- Splunk
- Active Directory
- Kali Linux's crowbar
- Ubuntu
- Atomic Red Team

## Network Diagram
<img src="https://imgur.com/VsW4P2P.jpg" width="500" />

*Ref 1: Network Diagram*

All four VMs are in the same 192.168.100/24 network and are hosted in Virtualbox.  

### Virtualbox NAT Network Settings


<img src="https://i.imgur.com/JRGOMqZ.jpg" width="500" />

*Ref 2: NAT Networks ADProject configuration*

ADProject has a IPv4 Prefix of 192.168.100.0/24.

<img src="https://i.imgur.com/0FSsjvl.jpg" width="500" />

*Ref 3: Windows 10 NAT Network*

All VMs are attached to NAT Network and still have internet access. In *Ref 3*, only the settings for Windows 10 is shown.

### 
